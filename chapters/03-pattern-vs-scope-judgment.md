# Chapter 2 — What You're Actually Good At (And What `gh copilot suggest` Is Better At)

> Pattern completion is the CLI's domain. Scope judgment is yours. Knowing which is which is the whole game.

---

## Learning outcomes

1. **(Understand)** Distinguish pattern completion (where the CLI is superhuman) from scope judgment (where the CLI is structurally blind).
2. **(Apply)** Classify a set of terminal tasks as CLI work or human work.
3. **(Analyze)** Identify the specific supervisory capacity being exercised at a given step in a shell build.

---

## Opening

Seth needed to find large files on his laptop. The disk was full and he wanted to know what was taking up space.

He typed `gh copilot suggest "find large files in my home directory"`. The CLI generated:

```bash
find ~ -type f -size +100M -exec ls -lh {} \;
```

It is a perfectly correct `find` command. The syntax is right. The flags are appropriate. The combination is idiomatic. If Seth had typed it by hand, it would have taken him minutes — looking up `-size`, remembering whether `+100M` was "more than 100 megabytes" (it is), figuring out how to print the file sizes alongside the paths. `gh copilot suggest` produced it in two seconds.

Then Seth ran `gh copilot explain` on it. The explanation walked through each flag and confirmed what the command would do.

Then Seth stopped.

The CLI had produced a command that did exactly what he asked. The CLI had no idea what *Seth meant by large* in his context. Did "large" mean files over 100 megabytes? The CLI guessed 100M because that is a common threshold. But Seth was actually looking for files over 1 GB — he had run into the disk-full warning at the 50 GB free mark and he needed to find the multi-gigabyte files, not the hundred-megabyte ones. The CLI's 100M threshold would return hundreds of files and bury the actual culprits.

The CLI also had no idea which directories to exclude. Seth's home directory contained a `node_modules/` that was 3 GB by itself, mostly dependencies that were not the culprit (he could not just delete `node_modules/` because the projects would break). It contained a Time Machine backup directory. It contained a Docker image cache. The `find` would dig into all of them and return their contents as "large files," obscuring the actual answer.

The CLI had also no idea what Seth was going to *do* with the answer. If Seth was going to delete the files, then a `find` with `-exec rm` would be the natural next step. If Seth was going to investigate which projects were producing the most data, he needed the files grouped by parent directory, not a flat list. The CLI's flat list was the right shape for one use and the wrong shape for the other; the CLI did not know which Seth wanted.

That gap — between the command being *syntactically perfect* and the command being *the right command for this task in this context* — is the chapter.

<!-- → [TABLE: Division of labor — two columns: CLI does / Human does. CLI: pattern completion, syntax generation, flag lookup, command structure. Human: scope definition, target specification, exclusion decisions, intent verification, consequence assessment.] -->

---

## What pattern completion is, and why the CLI is superhuman at it

The CLI was trained on an enormous corpus of shell commands. It has seen `find` invocations in millions of variations. It has seen `-size +100M`. It has seen `-exec ls -lh {} \;`. It has seen these patterns combined and recombined across decades of shell usage on Stack Overflow, GitHub, blog posts, documentation, and engineering chat logs.

When you type "find large files," the CLI completes the pattern. It assembles the most-probable shell idiom for "find files matching some size threshold" and returns it. The completion is informed by every variation of that pattern it has ever seen. The result is, on average, more idiomatic and more correct than what a human would produce from memory — because the human has only used `find` a few hundred times, and the CLI has effectively used it a few million.

This is the CLI's domain. It is faster than you. It will stay faster than you. The gap will widen, not close. There is no horizon on which a human will be a better source of well-formed shell command syntax than a frontier-generation language model. The right strategy is not to compete here; it is to recognize where the CLI's pattern completion ends and your work begins.

Pattern completion does well:
- **Syntax.** The exact flag names, the right argument ordering, the proper escape characters.
- **Idiom.** The conventional way to combine commands. The expected pipe chain. The standard subshell pattern.
- **Recall.** What `-mtime +7` means, what `xargs -0` does, when to use `[[ ]]` vs. `[ ]`. The CLI has read more shell than you ever will.
- **Translation.** Natural-language descriptions to shell commands. The CLI is genuinely good at this in a way that would have seemed implausible five years ago.

Pattern completion does not do:
- Any of the work in the next section.

---

## What scope judgment is, and why the CLI is structurally blind to it

**Scope judgment** is the work of deciding which files, which directories, which conditions, which consequences are the ones that should apply *in this particular case*. It is the work that requires knowledge of your filesystem, your project, your intent, and your tolerance for being wrong.

The CLI cannot do scope judgment, not because it is bad at it but because it does not have the information. The CLI cannot see:

- **Your filesystem.** The CLI does not know what is in `~/Downloads/`. It does not know whether `~/projects/old/` contains an active project or three years of archived experiments. It does not know whether `~/Documents/` has a backup that you would lose if you ran `rm -rf` on it.

- **Your git history.** The CLI does not know which branch you are on. It does not know whether `main` is protected. It does not know whether the commits you are about to rebase have been pushed to a shared remote. It does not know whether anything in your working directory is uncommitted.

- **Your intent.** The CLI does not know what you mean by "large" or "old" or "clean up." It does not know whether you are about to delete the files or just look at them. It does not know whether you are testing or running for real.

- **The consequence horizon.** The CLI does not know whether the script you are writing is for a one-off cleanup or for production automation that will run nightly. It does not know whether being wrong costs you five minutes or five days.

None of this is in the prompt. None of this is in the model's training. None of this is recoverable from any context the model has access to. The CLI is operating on an enormous body of shell knowledge and zero knowledge of *your specific situation*. The completion it produces is the most-probable answer to your prompt averaged across all the people who might have typed it. You are not the average. The gap between the average and you is the scope-judgment gap.

The CLI is **structurally blind** to this work. Not blind because the model is too small, or because the training data is too narrow. Blind because the information required to do scope judgment is not — and cannot be, without revealing things you would not want revealed — in the prompt.

What scope judgment requires (and only you can supply):

- **Which files matter.** Of all the files that match the pattern, which ones do you actually want the command to act on?
- **What to exclude.** Which directories or patterns must the command *not* touch? `node_modules/`? `.git/`? Backup directories?
- **What the consequence horizon is.** Is this a recoverable operation if wrong? An irreversible one?
- **What "done" looks like for *this* task.** Is a flat list of files enough, or do you need them grouped? Sorted? Annotated?

---

## The solve-verify asymmetry

There is a related asymmetry worth naming explicitly, because it determines how you should spend your attention during a `gh copilot suggest` session.

**The CLI solves faster than you can verify.** It generates a command in two seconds. Verifying that the command is right for your situation — reading the explanation, considering the scope, checking the directories that would be affected — takes you longer. This gap is not narrowing. As CLI assistants get faster and more capable, the gap widens. The CLI generates more, faster; your verification capacity is roughly stable.

The asymmetry has an operational implication. Your time at the terminal during AI-assisted work is *almost entirely* verification time. The generation is essentially free; the verification is the bottleneck. The right way to think about your role is not as a faster typist — the CLI is already a faster typist than you. The right way is as a *checker* whose attention is the scarce resource.

This reframes what good terminal practice looks like. The student who tries to keep up with the CLI's generation pace by skimping on verification is operating against the asymmetry. The student who slows down at the verification step — running explain, checking the scope, predicting the consequence — is using their scarce resource where it matters.

<!-- → [DIAGRAM: The solve-verify asymmetry — timeline. Codex's solve speed increasing over time. Human verification capacity stable. The gap widens. The human's job: not to solve faster but to verify better.] -->

---

## The dangerous middle

There is a third category of work that neither *pure pattern completion* nor *pure scope judgment* fully describes. Tasks that *look* like pattern work — the kind of task you would expect `gh copilot suggest` to handle — but that *require* scope judgment to do correctly.

These are the **dangerous middle**.

A `find` to "clean up old logs" looks like pattern work. The CLI knows the syntax. It will return a `find` command that runs. But the *definition* of "old" is scope judgment that only you can supply. If you accept the CLI's default (`-mtime +7`, files modified more than 7 days ago), you may sweep up files that are old in the file-system sense but active in the project sense.

A `git filter-branch` to "remove the secret from history" looks like pattern work. The CLI knows the syntax. It will return a `git filter-branch` invocation that operates on commits matching some criterion. But the *criterion* — which commits, on which branches, with which consequences for collaborators — is scope judgment.

A `chmod` to "fix the permissions" looks like pattern work. The CLI will return a `chmod` invocation. But what the right permissions *are* for your project depends on context the CLI does not have.

The dangerous middle is dangerous because the CLI's output *looks complete*. The command runs. The exit code is zero. The scope-judgment gap is invisible until something downstream breaks. Most of the silent failures in Chapter 1 happened in the dangerous middle.

Chapter 9 owns the dangerous middle as its full chapter. For now, recognize the shape: tasks where pattern completion *gets you most of the way* and *misses the part that matters*. Those are the tasks where the suggest → explain → verify gate is most essential, because the verify step is the only thing that catches the gap.

---

## Worked example: the same task analyzed twice

The task: archive log files older than 7 days from a project directory to an `archive/` folder.

### Run one — Codex unattended

Seth types:

```
gh copilot suggest "archive log files older than 7 days from my project to archive/"
```

The CLI returns:

```bash
find ~/projects/my-project -name '*.log' -mtime +7 -exec mv {} archive/ \;
```

Seth runs it. Exit 0. The CLI moved 47 files. Seth assumes that is roughly right — the project has been running for a while, and 47 sounds plausible. He moves on.

Three days later, Seth's build breaks because some of the moved files were active logs that the build was still appending to.

The scope-judgment work that was skipped: deciding what *older than 7 days* meant in this project (active logs vs. modification time), deciding which directories to exclude (`.git/`, `node_modules/`), deciding whether to verify the file list before moving.

### Run two — Codex with the supervisory capacities exercised

Seth types the same prompt. The CLI returns the same command. Then Seth pauses and runs through the five capacities (formally introduced in Chapter 5; informally applied here).

**Plausibility auditing.** Does this command match what Seth knows about his project? The `-mtime +7` will catch files modified more than 7 days ago. Seth knows that some of his project's log files have not been *modified* in 8 days but are being actively appended to by a long-running process. Plausibility audit fires: the command's threshold does not match the project's notion of "active."

**Problem formulation.** What is Seth actually trying to do? Archive logs that are *no longer being used*. That is different from "files older than 7 days." Seth reformulates: "files in `~/projects/my-project/logs/` that are *.log* and whose process is not running."

**Tool orchestration.** Is `gh copilot suggest` the right tool for this revised problem? It is — the new problem is still pattern-completion-shaped — but the criterion needs to be different. Seth issues a more specific suggest: *"find .log files in my project's logs directory that are not currently held open by any process."*

The CLI returns:

```bash
lsof +D ~/projects/my-project/logs | awk 'NR>1 {print $NF}' | sort -u > /tmp/active_logs.txt
find ~/projects/my-project/logs -name '*.log' | grep -v -F -f /tmp/active_logs.txt
```

This finds files held open and excludes them from the candidate list. Seth runs `gh copilot explain` on the second command to confirm the `grep -v -F -f` behavior (it does what he thinks — invert match, fixed strings, from a file).

**Interpretive judgment.** The command returns 31 files. Seth's project has been running about 6 weeks; 31 log files for that period sounds right. He skims the list. They are all from the older logs subdirectory; none from the active subdirectory. Plausibility audit passes.

**Executive integration.** Seth's original goal was to move the files to `archive/`. The candidate list is now correct; he can compose the `mv`:

```bash
find ~/projects/my-project/logs -name '*.log' | grep -v -F -f /tmp/active_logs.txt | xargs -I {} mv {} archive/
```

He runs `gh copilot explain` on this. He runs it with `echo mv` first as a dry-run. The output looks right. He runs the real `mv`. Exit 0. 31 files moved. No active logs were touched.

Same CLI. Same starting prompt. Different outcome.

The lesson: pattern completion produced the syntactically perfect command on both runs. Scope judgment was the difference between an outcome that broke the build and an outcome that did not.

---

## Common misconceptions

**"With more context in my prompt, the CLI can do scope judgment."** Partially. A longer prompt helps the CLI guess your scope more accurately. It still cannot see your filesystem. It still cannot read your intent. More context narrows the most-probable answer; it does not eliminate the gap between the most-probable answer and the right answer for *your* situation.

**"`gh copilot ask` can investigate the situation for me."** `gh copilot ask` is useful for problem formulation — interrogating the task before suggesting anything. It is less useful for scope verification, because the *ask* version of the question requires the same context the *suggest* version requires. You have to supply it either way.

**"This whole framing is just about being careful. I am careful."** Carefulness is necessary, not sufficient. The chapter is not arguing you should be more careful; it is arguing that *carefulness needs a structure* — the explicit naming of which capacities are operating and where — to be reliable. Vague carefulness misses the dangerous middle.

**"I can spot the dangerous middle when it happens."** Sometimes. The whole point of *silent* failure is that you cannot reliably spot it without a discipline. The discipline is what makes the spotting reliable.

**"Pattern completion will get good enough that scope judgment isn't a separate skill."** It will not. Pattern completion is about *averages over the corpus*; scope judgment is about *your specific situation*. The two are categorically different. Improving pattern completion does not give the model access to your filesystem.

---

## Exercises

1. **(Apply)** Given the following ten terminal tasks, classify each as **CLI work** (pure pattern completion), **human work** (pure scope judgment), or **dangerous middle** (pattern work with scope-judgment hazard). Defend each classification.
   - Renaming all `.txt` files in a folder to `.md`.
   - Deleting all branches in your git repo that have been merged to main.
   - Counting the number of lines in a CSV.
   - Finding the largest files on your laptop.
   - Writing a `cron` entry to run a script every Monday at 7am.
   - Setting up SSH agent forwarding for a remote server.
   - Removing a committed API key from your git history.
   - Listing all files modified in the last week.
   - Setting executable permissions on a script.
   - Archiving last semester's project directory.

2. **(Analyze)** Read the following provided CLI transcript (a real or constructed one). At each step, identify the moment where scope judgment should have been exercised but was not. Trace what went wrong as a result.

3. **(Create)** Write your own labor separation rule for a shell project you are currently working on. The rule should be specific to the project: name which kinds of operations you will delegate to the CLI without explicit scope-checking and which kinds you will always scope-check. Keep the rule short enough to remember.

---

## What would change my mind

The chapter's strong claim is that **the pattern-completion / scope-judgment division is categorical, not gradient** — that no improvement in pattern completion makes scope judgment a different problem. If a 2027 or later system could demonstrate reliable scope judgment in domains where it does not have access to the user's filesystem or git history — perhaps through some form of context inference that I am not anticipating — the categorical framing softens to gradient. The chapter would still hold (scope judgment is much weaker than pattern completion at the CLI), but the framing would be "the CLI is bad at scope judgment now and getting better" rather than "the CLI is structurally blind."

I think this is unlikely on the next-edition timeline because the structural argument (the model does not have the information) is hard to engineer around without giving the model filesystem access — which has its own dangers and is not the path most agentic-coding tools are taking.

---

## Still puzzling

- **Where exactly does pattern completion shade into scope judgment?** The chapter draws the line crisply for purposes of teaching. In practice, the line is fuzzier. Some tasks have *some* scope work (knowing roughly which directories) the CLI can guess at, and *some* scope work (which specific files matter) the CLI cannot. Where the fuzziness sits varies by task.

- **Does the asymmetry hold for agentic CLI surfaces that can read files?** The post-January-2026 `copilot` CLI in plan mode can read the project directory before generating. This narrows the scope-judgment gap somewhat. Whether it narrows enough to change the chapter's argument is open; the book's working answer is no (intent is still not in the filesystem), but the answer requires watching the surface evolve.

- **Are there individual variations in scope-judgment skill?** Some terminal users are systematically better at scope judgment than others. Whether this is teachable or partly innate is open. The chapter assumes teachable; the book's discipline depends on the assumption.

---

## AI Wayback Machine

🕰️ **Lillian Moller Gilbreth** (1878–1972) — American industrial-organizational psychologist and engineer whose work on time-and-motion studies and ergonomics produced the modern field of human factors. Working with her husband Frank Gilbreth in the early 20th century, she developed systematic methods for distinguishing the parts of a task that suited the human (judgment, perception, adaptation) from the parts that suited mechanical execution (repetition, precision, speed). After Frank's death she expanded the work into a humane version of scientific management that emphasized *which cognitive work belongs to which agent* and built the framework most contemporary task-allocation thinking still uses.[^1] Gilbreth's question — what does the human do that the machine cannot — is exactly this chapter's question for AI-assisted terminal work. The Gilbreths' answer for industrial work in 1920 was that humans handled perception, judgment, and adaptation while machines handled execution. The answer for AI-assisted terminal work in 2026 is structurally similar: humans handle scope judgment, intent verification, and consequence assessment; the CLI handles pattern completion. The form is the same; the surface is different.

*(The TIKTOC originally specified Frederick Winslow Taylor here. The pantry research recommended swapping to Gilbreth on grounds of both substantive fit — her work on human-machine task allocation is closer to the chapter's argument — and diversity. Lillian Gilbreth is the chapter's figure.)*

---

## Bridge

You can name the capacities. Chapter 3 explains why school is not teaching them and why, at the terminal specifically, the technically fluent student is on their own.

---

[^1]: Gilbreth, L. M. *The Psychology of Management*. Sturgis & Walton, 1914. See also Gilbreth and Gilbreth, *Applied Motion Study* (1917) and *Time Study and Motion Study as Fundamental Factors in Planning and Control* (1916).
