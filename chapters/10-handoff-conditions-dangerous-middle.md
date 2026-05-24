# Chapter 9 — Handoff Conditions and the Dangerous Middle

> Not "it ran without errors." A specific, testable condition that must be true before the next step begins — because the terminal's silent failure mode is the most dangerous one.

---

## Learning outcomes

1. **(Understand)** Explain what a handoff condition is for a terminal build and why exit 0 fails as one.
2. **(Apply)** Write handoff conditions for a set of provided terminal tasks.
3. **(Analyze)** Identify the dangerous middle — where `gh copilot explain` describes accurately, exit 0 confirms success, and the command did the wrong thing.

---

## Opening

Seth approved a `gh copilot suggest` output. The explain step had walked through the command. The output had read as accurate. The handoff condition Seth had written was "the command exits 0."

The command exited 0. Seth proceeded to the next step.

Six days later, downstream of three subsequent build steps, something broke. Seth traced backward. The original command had moved files matching `*.log` from `~/projects/my-project/logs/` to `~/archive/`. The exit-zero confirmed the move. What the exit-zero did *not* confirm: that the files moved were the ones Seth intended. Buried in `~/projects/my-project/logs/` was a `partner-vendor/` subdirectory containing `.log` files that were technically logs in the file-system sense but were *active vendor configuration* that the project depended on. The `find` had descended into the subdirectory. The vendor logs were now in `~/archive/`. The project had been running on default values for six days because the configuration was missing.

The handoff condition Seth had written was met. Exit 0. The condition Seth had *needed* to write was *"the vendor configuration directory has not been touched"* — which would have required him to know the vendor configuration directory existed *as such* and to formulate the condition. He did not know. He did not write the condition. Six days of partial defaults followed.

This is the **dangerous middle**: the case where the `gh copilot explain` is accurate, the exit code is zero, the handoff condition is met *as written*, and the command did something Seth would not have approved if he had known to check. The condition that would have caught it was a condition Seth did not know to write.

This chapter is the dangerous middle, named.

![The handoff condition as a gate between build](images/10-handoff-conditions-dangerous-middle-fig-01.png)
*Figure 10.1 — The handoff condition as a gate between build*

---

## What a handoff condition is

A handoff condition is a binary check between build steps. After Step N completes, before Step N+1 begins, the check answers: *is the state of the system what Step N was supposed to produce?*

Three properties.

**Specific.** Not "looks right." Not "no errors." Something with a path, a count, a value that can be checked in seconds.

**Testable.** A check you can run and that produces an answer. A `ls`. A `find -print`. A `wc -l`. A `git status`. An `echo $?`. The check itself is a command; its output is the verdict.

**Binary.** Pass or fail. Yes or no. "Mostly worked" is not a handoff condition.

For each major operation in a build, write at least one handoff condition. The condition is written *before* the operation runs — so that the operation's results can be checked against it. Writing the condition after the operation, against the results, is post-hoc validation, which catches some failures but not the ones where the results look fine and are not.

Strong vs. weak handoff conditions for terminal tasks:

| Operation | Weak condition | Strong condition |
|-----------|----------------|------------------|
| Log archive (`find` + `mv`) | Exit 0 | Exactly N `.log` files moved to `~/archive/`; source directory contains 0 `.log` files older than 7 days; no files moved outside `~/archive/`; the `partner-vendor/` subdirectory still contains its original files |
| Git rebase | No merge conflicts | All N feature-branch commits present in rebased branch (by SHA or by commit-message check); commit count matches pre-rebase count |
| Deploy to server | `rsync` exits 0 | Server URL serves version SHA equal to current local HEAD; `index.html` mtime within 30 seconds of local; no 404s on representative page links |
| Permissions change | No error | Exactly the intended file paths have mode 644; no other paths modified; `find -perm` confirms |
| Cleanup script | "Cleanup ran" | List of deleted files printed; no file deleted outside `~/tmp/`; the `~/keep-me/` directory still contains its original files |

The strong conditions are not heroic. They are concrete. Each takes thirty seconds to verify. Each would have caught a class of failure the weak condition misses.

The Chapter 4 gate (suggest → explain → verify) ends with *verify*. The handoff condition *is* the verify step, made operational and persistent. The gate's verify is the moment-of-execution check; the handoff condition is the check that survives into the next step's beginning.

---

## What the dangerous middle is

The four categories of terminal silent failure from Chapter 1:

1. **Visible failure.** Non-zero exit. Easy to catch.
2. **Silent wrong scope.** Exit zero. More (or fewer) files affected than intended. *Easier* to catch with strong handoff conditions.
3. **Silent wrong target.** Exit zero. Right operation, wrong files. Caught by handoff conditions on the *file identities*, not just counts.
4. **Silent wrong meaning.** Exit zero. Right command. The command did what it was supposed to do, and what it was supposed to do was not what you needed.

The dangerous middle is shape 4. It is the failure where:

- The `gh copilot explain` was *accurate* about the command.
- The command did *exactly* what the explain said.
- The handoff conditions you wrote were *met*.
- The command was still wrong, because the condition you needed was one you did not think to write.

This is the failure mode the chapter exists for. The first three shapes are catchable by progressively strong handoff conditions. The fourth is catchable only by *plausibility auditing* (the PA capacity from Chapter 5) on the output and the situation together — and PA's reliability depends on whether you have the domain knowledge to recognize when something is off.

The vendor-configuration directory in the chapter opening was a dangerous middle. The command moved files. The files matched the pattern. The exit was zero. The handoff "exit 0" was met. Seth did not know the vendor configuration directory existed; he could not have written a condition for it. The plausibility audit that *could* have caught it would have been "wait, the archive directory contains a `partner-vendor/` subdirectory that I do not recognize — let me investigate before proceeding." Seth did not run the audit because the previous five archives had been routine and he had stopped looking.

---

## Common dangerous-middle patterns in terminal work

A taxonomy worth knowing.

**`find` that matches more than intended.** The classic. Patterns like `*.log` match in subdirectories you did not know existed. `-mtime +7` means "modified more than 7 days ago" — files not modified but actively read are included. `-exec rm` operates on the matched set. The strong handoff condition is "the printed file list (run `find -print` first as a dry-run) is the set I expected."

**`rm` with a glob that expands unexpectedly.** `rm *.bak` from the wrong directory. `rm $VAR/*` where `$VAR` is unset (expands to `rm /*`). The strong handoff condition is "the glob is dry-run-printed first; the path expansion is checked; the command is not run until the printout matches expectation."

**`git` that operates on the wrong branch.** `git reset --hard` on a branch with uncommitted work. `git push --force` to `main`. `git rebase` against a remote branch that has shared history. The strong handoff condition is "the current branch is `git branch --show-current`; the operation's consequences are stated for *this branch* before the command runs."

**`sed` or `awk` that modifies in-place without backup.** `sed -i 's/foo/bar/' *.txt` is one keystroke from `sed -i 's/foo/bar/' *.txt.bak` (the wrong wildcard) or from `sed -i.bak 's/foo/bar/' *.txt` (the right way, with backups). The strong handoff condition is "the change is checked against a known sample input first; backups are present or explicitly disabled with awareness."

**Pipeline that fails silently mid-stream.** `cmd1 | cmd2 | cmd3` where `cmd1` produces nothing (because of an error) and `cmd2` happily processes the empty input. Pipelines do not propagate non-zero exits by default. The strong handoff condition is "intermediate output of each pipeline stage is non-empty when expected to be non-empty; `set -o pipefail` is set in any script."

The CLI does not warn about these patterns when generating commands. The `gh copilot explain` mentions what the command does, not what it might do to your specific filesystem. The handoff conditions you write are the protection.

| Item | Meaning |
| --- | --- |
| Strong vs. weak handoff conditions for terminal tasks. Five examples. Left: weak (exit 0, no errors). Right: strong (specific count, specific files, specific state). | The pattern becomes easy to misuse or overlook. |

---

## When a handoff condition fails: revert, do not correct forward

A discipline from the conducting framework: if a handoff condition fails, do not patch the result with a follow-up command. Revert. Respecify.

The reasoning: forward correction pollutes the context. Your terminal history now contains the failed command, the partial state, the patch, and the patched state. Future you (or future Copilot, if you continue using suggest) will be operating against this mess. The mess is harder to reason about than the clean state.

The discipline:

1. **First correction:** OK. The CLI's next suggestion has the failed output in context; the suggestion may be appropriately revised.
2. **Second correction:** The context is getting cluttered. The CLI's third response has both your failures in context; the quality drops.
3. **After two corrections:** Stop. `/clear` the session if your tool supports it. Start fresh. Write a *better specification* — one that includes the failed condition as a negative constraint, so the new attempt cannot reproduce the failure.

OpenAI's own engineers describe the rule for their tool (Codex) the same way: *"If you've corrected Codex more than twice on the same issue, the context is cluttered. Start fresh with a more specific prompt."*[^1] The rule transfers to `gh copilot suggest`. After two failed corrections, restate.

The cost of starting fresh feels high. It is almost always lower than the cost of continuing with a polluted context.

---

## Common misconceptions

**"Exit 0 is a strong condition."** No. It says the process completed. It says nothing about whether the process did what you meant.

**"I'll catch the dangerous middle by being careful."** Vague carefulness misses the dangerous middle. The discipline of *writing the conditions in advance* is what catches it.

**"My terminal history is full of un-conditioned commands; I've been fine."** Survival bias. The commands that have not yet exposed a dangerous-middle failure are the ones whose failures have not yet surfaced. By the time you find out, the cost is downstream.

**"Forward correction works for me."** Sometimes, on small fixes. The math turns against you on multi-step builds. After three forward corrections, the context is unrecoverable; the revert-and-respecify costs less.

**"PA always catches the dangerous middle."** Sometimes. PA fires when you have the domain knowledge to notice. For domains you are new to, PA is less reliable. Handoff conditions are the protection that does not require pre-existing domain mastery.

---

## Exercises

1. **(Apply)** Write handoff conditions for a five-step terminal build you are working on. Each condition: specific, testable, binary. Test each by deliberately introducing the failure the condition is meant to catch.

2. **(Analyze)** Given a provided CLI transcript that contains a dangerous-middle failure, identify (a) which step produced the failure, (b) what handoff condition would have caught it, (c) why the condition was likely not written.

3. **(Create)** Apply the revert-and-respecify rule to a recent build where you used forward correction. Describe what the build would have looked like with the rule applied. Compare to what actually happened.

---

## What would change my mind

The chapter's strong operational rule is **revert-and-respecify after two failed corrections** for the conducted build. If a controlled comparison found that engineers who used forward correction produced builds of equivalent quality to engineers who reverted-and-respecified after two failures, the rule weakens to a preference. The book would still recommend the discipline; the case for "after two, stop and restart" softens.

The chapter operates on OpenAI's own published guidance, on the structural argument about context pollution, and on the convergent intuition of senior engineers in the practitioner literature.

---

## Still puzzling

- **The exact threshold for "stop and restart."** Two is the book's number. Some practitioners use three. Some use one (any failure → restart). The right number probably depends on session length, model generation, and personal practice.

- **How much of the dangerous middle is catchable by hooks** (the equivalent of `git pre-commit` hooks at the level of `gh copilot suggest`). Some agentic tools (Claude Code, Codex) have hook systems that can intercept generated commands before execution. `gh copilot suggest` does not. Whether such hooks will arrive for `gh copilot` and whether they would meaningfully reduce dangerous-middle incidence is open.

- **Whether plausibility auditing develops with the discipline.** The book's working answer is yes — practicing PA on every output strengthens it over time. Whether the strengthening is measurable, and at what rate, is not directly studied for terminal AI.

---

## AI Wayback Machine

🕰️ **Grace Hopper** (1906–1992) — computer scientist and US Navy Rear Admiral who developed COBOL and the A-0 compiler and who insisted, across her career, that "done" must be *defined* before it can be verified. Hopper's account of programming was that the practitioner's discipline is in *specifying correctness explicitly* — not assuming that the absence of errors equals correctness. She wrote: *"The most dangerous phrase in the language is 'we've always done it this way.'"*[^2] The dangerous middle is exactly the failure mode that "we've always done it this way" produces — the command that ran fine before is run again, the condition that was met before is checked again, and the case where the unchecked condition matters is the case where the failure occurs. Hopper's insistence on explicit verification criteria is the handoff condition principle stated at the founding of software engineering. The chapter's discipline is hers, restated for `gh copilot suggest` and applied with care.

---

## Bridge

You have the full conducting discipline for terminal builds. Chapter 10 applies the same discipline to creative work — scripts whose output is read by humans and whose aesthetics matter.

---

[^1]: OpenAI engineers, "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor" (forum.openai.com, December 4, 2025). The rule is for Codex specifically; the same logic applies to `gh copilot suggest`.
[^2]: Hopper, G. *Various lectures and interviews*, 1980s. The "we've always done it this way" phrase is quoted across her recorded talks and is widely attributed.
