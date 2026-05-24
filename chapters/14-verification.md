# Chapter 13 — Verification: How You Know It Works

> The build is done when it passes the handoff conditions — not when `gh copilot suggest` says it's done, not when it exits 0.

---

## Learning outcomes

1. **(Apply)** Run a structured verification pass on a completed terminal build using explicit criteria from the problem formulation.
2. **(Analyze)** Distinguish build failures from silent failures that look like successes.
3. **(Evaluate)** Produce a post-build assessment.

---

## Opening

Seth's repository-hygiene build had finished its last step. The summary printed. The script reported having cleaned three branches and recovered 4.7 GB of disk space. Exit 0.

Seth was about to declare it done.

Then he ran the verification pass — the one he almost skipped because the script's own report had felt convincing. He checked the file system. The three branches he had intended to clean were clean. The active branch was unchanged. The preserved-snapshot branch (caught by plausibility audit in Chapter 12) was untouched.

He also checked one thing the script's report had not addressed: he ran `git status` on each of the cleaned branches by checking them out one by one. On the first cleaned branch, `git status` reported a deleted file: `export_presets.cfg`. The script had removed `export_presets.cfg` as part of cleaning the branch.

That was not in the spec. The spec said to remove `.godot/imported/`, `.import/`, `addons/.cache/`, `builds/android/`. Nothing about removing `export_presets.cfg`.

Seth investigated. The `export_presets.cfg` had been deleted because the `rm -rf` was matching against a glob pattern that the CLI had generated as `*.{cfg,import}` in the cache directories — but the glob had expanded across the entire branch's worktree, not just the cache paths. The `export_presets.cfg` file at the project root happened to match. The script's report did not mention it because the report only listed the *directories* it had cleaned; the auxiliary deletion was invisible in the summary.

The script had passed every handoff condition Seth had written. The plausibility audit during execution had not fired. The error surfaced only in the verification pass — when Seth checked something the script had not been asked to report on.

This is what verification is for. The condition you did not think to check at the per-step level surfaces at the end. The build is not done when the last step's handoff passes. The build is done when the verification pass confirms the *final state of the system* matches what the formulation said it would.

![The verification sequence ](images/14-verification-fig-01.png)
*Figure 14.1 — The verification sequence *

---

## The three verification passes

The verification step has three layers, run in order.

**Pass 1: mechanical verification.** Did each step complete without errors? Exit codes zero? Expected output present? Nothing in stderr that looks like a problem? This pass is the one most students stop at. It is the necessary first check; it is not sufficient.

**Pass 2: scope verification.** Did the build touch the right files and *only* the right files? Are the counts what you expected? Are the files identified by name (not just by count) the ones you intended? This is the pass that would have caught Seth's `export_presets.cfg` deletion — by checking `git status` on each cleaned branch rather than trusting the script's own report.

The mechanical: how to do Pass 2. For each file-modifying step, you can usually verify scope by running a `find` or `ls` or `git status` *after* the step against the directory the step touched. The output of the post-hoc check should match your expectation; surprises are caught here.

**Pass 3: intent verification.** Does the final state of the system match what the *formulation* (Chapter 7) said you wanted? Not what the spec said, not what the steps did, but what the original problem formulation specified as the goal.

The repository-hygiene build's formulation said: "Remove generated artifacts (`.godot/imported/`, `.import/`, `addons/.cache/`, `builds/android/`) from non-active branches, preserving the active branch's state." The intent-verification check: are *generated artifacts* (defined: the four directories — explicit list) removed from non-active branches? Is the active branch state preserved? The answer should be yes on both. The `export_presets.cfg` deletion fails the intent-verification because `export_presets.cfg` is not in the generated-artifacts list — it is configuration.

The three passes are *cumulative*. Pass 2 catches things Pass 1 missed. Pass 3 catches things Pass 2 missed. Stop at Pass 1 and you ship the build that passed `exit 0` and is wrong.

---

## The post-build learning document

The verification pass produces *what happened*. The post-build learning document is the artifact that records *what you learned from what happened*.

Five sections:

1. **What I built.** One paragraph, plain language. The kind of description you would give a friend who asked what you spent the afternoon on.

2. **What I delegated to `gh copilot suggest` and why.** The specific work the CLI did, with the why. The cognitive labor split made explicit.

3. **What I kept for myself and why.** The mirror. The work that was irreducibly yours, with the why. The supervisory work, named.

4. **What I learned that I didn't know before.** The discoveries. The features of the shell you understand better now than you did this morning. The CLI behaviors you learned to anticipate. The CLI.md entries you added.

5. **What I would do differently.** The honest section. A specific decision you would reverse. The thing you would change in the formulation, in the plan, in the CLI.md, or in the build's execution.

The document is one page. It takes thirty minutes to write. It is the artifact that converts the experience of building into the capacity to *teach* building — or, more honestly, into the capacity to do the next build better than this one.

The discipline is to write it *now*, while the lessons are fresh, not later. By tomorrow the specifics will be fuzzy; by next week they will be gone.

---

## Worked example: the repository-hygiene build's verification

Seth's verification of the build that opened the chapter.

**Pass 1 (mechanical).** Every step exited zero. The summary script printed the expected output (branches cleaned, space recovered). No errors in stderr. **PASS.**

**Pass 2 (scope).** Seth checks each cleaned branch by checking it out and running `git status`:
- Branch A: cleaned. `git status` shows untracked `export_presets.cfg`. UNEXPECTED.
- Branch B: cleaned. `git status` clean. Expected.
- Branch C: cleaned. `git status` shows untracked `export_presets.cfg`. UNEXPECTED.
- The active branch: `git status` clean. Expected.
- The preserved-snapshot branch: `git status` clean. Expected.

**Pass 2 outcome: PARTIAL FAIL.** Two branches have unexpected deletions.

**Pass 3 (intent).** The formulation specified removal of generated artifacts. `export_presets.cfg` is not a generated artifact; it is configuration (Godot's export targets and signing config live there). The deletion violates the formulation's intent.

**Pass 3 outcome: FAIL.**

The build is not done. The fix:

1. Restore the deleted `export_presets.cfg` files on Branches A and C from the most recent commit (`git checkout HEAD -- export_presets.cfg`).
2. Update the script's spec to scope the `rm` to *directory paths under the cache directories*, not glob-by-extension across the branch.
3. Re-run the script on the affected branches with the corrected spec.
4. Re-run Pass 2 and Pass 3 to confirm.

The post-build document, after the fix:

> **What I built.** A bash script that cleans generated artifacts (`.godot/imported/`, `.import/`, `addons/.cache/`, `builds/android/`) from non-active branches of a Godot git repo while preserving the active branch's state.
>
> **What I delegated.** The `git for-each-ref` survey logic, the `git checkout` + `rm -rf` sequence, the disk-space arithmetic. Pattern-completion work where the CLI is faster than I am.
>
> **What I kept.** The decision about which branches to skip (the preserved-snapshot was caught by plausibility audit). The formulation of "generated artifacts" (which the first run got wrong by interpreting it too broadly). The verification pass that caught the `export_presets.cfg` deletion. The supervisory work in general.
>
> **What I learned.** That globs in `rm -rf` can match across a branch's worktree more broadly than I intended; that the CLI's interpretation of "generated artifacts" defaults to file extensions rather than directory paths; that the verification pass needs to check `git status` per branch, not just trust the script's own report. CLI.md updated with the lesson about glob scoping.
>
> **What I would do differently.** Write the spec for Step 3 with *directory paths*, not glob patterns. Add a Pass 2 check that runs `git status` on each cleaned branch automatically (not as a separate manual step). Include the per-branch status check in the script's output so I do not have to run it separately.

The document is roughly 200 words. It is the artifact Seth will reference the next time he builds something like this. The lessons-learned section of CLI.md has been updated with the glob-scoping insight. The next build starts from a better place.

---

## What the verification pass cannot catch

A few things the discipline does not promise.

**The verification pass cannot catch failures whose downstream effects have not yet surfaced.** A build that introduces a subtle behavior change that only matters in a production scenario you have not yet tested will pass all three verification passes today and break in the production scenario tomorrow. The verification covers the cases you know to check; the long tail is what monitoring (and the next build's plausibility audit) catches.

**The verification pass cannot catch failures in dependencies.** The `gh copilot suggest` may generate a command that calls an external tool whose behavior has changed since the CLI's training. The command runs; the external tool produces unexpected output; the script that consumed it produces wrong-but-not-obviously-wrong results. The verification can catch this if you check the *meaning* of the output (Pass 3) and not just its presence (Pass 2).

**The verification pass cannot catch failures of formulation.** If the formulation itself was wrong (you wanted X and asked for Y), the verification will confirm Y happened. The mismatch surfaces only when you notice that the system does not do X. This is why the upstream formulation work (Chapter 7) matters more than verification — formulation is cheaper than verification, and prevents the failures verification cannot detect.

The verification pass is necessary, not sufficient. It is the last line of defense, not the only line.

---

## Common misconceptions

**"Exit 0 means done."** No. Mechanical verification is Pass 1 of three. The other two passes are where the real verification happens.

**"If the script's report says it worked, it worked."** No. The script reports what it was asked to report on. The verification pass checks what the script may not have been asked to report on.

**"Intent verification is just gut feeling."** No. Intent verification is comparison against the *written* formulation. The formulation exists in writing (Chapter 7); the verification compares the system state to the writing.

**"I'll write the post-build document later."** You will not. Write during. Memory degrades fast.

**"Post-build is bureaucratic."** Five sections, thirty minutes, one page. The artifact pays back every time you start a similar build. The bureaucratic feeling is the discipline's friction; the practitioner value is in the lessons consolidated.

---

## Exercises

1. **(Apply)** Run a three-pass verification on your repository-hygiene build (or your equivalent). Document each pass's result. For any pass that catches a failure, fix it and re-verify.

2. **(Analyze)** A test in your script passes but you are not sure it is testing the right thing (a Pass 2 failure that Pass 1 missed). Diagnose what the test is actually checking; rewrite it to check what you intended.

3. **(Create)** Write a post-build learning document for your Chapter 12 build. Five sections. Honest, especially the "What I would do differently" section.

---

## What would change my mind

The chapter's strong operational claim is that **the three-pass verification catches a meaningful share of failures** that single-pass mechanical verification misses. If a controlled comparison — same builds, with one-pass vs. three-pass verification — found no measurable difference in the rate of post-deployment failures, the second and third passes become optional. The chapter would still recommend them; the urgency softens.

I expect the difference to be substantial because Pass 3 (intent verification) is the only pass that catches failures whose handoff conditions were under-specified, and these failures are the most expensive ones.

---

## Still puzzling

- **When verification becomes worth automating.** The three passes are manual in the chapter. For builds run repeatedly (a daily cron, a weekly grading pass), automating Pass 2 (scope) and parts of Pass 3 (intent) is worth the upstream effort. The book leaves the automation to the practitioner; the discipline is what gets automated.

- **Whether the post-build document should be shareable.** Honest post-build documents contain self-criticism; sharing produces incentives to perform rather than to be honest. The book's working answer: write candidly for yourself; share redacted versions when useful.

- **The relationship between this chapter's verification and CI/CD.** Industry CI/CD is the institutional version of this chapter's three passes. Whether students should learn the per-build manual version first, then the automated CI/CD version, or whether they should start automated, is open. The book's stance: manual first, because the discipline is the point.

---

## AI Wayback Machine

🕰️ **Barbara Liskov** (born 1939) — computer scientist whose work on behavioral subtyping and formal specification formalized the principle that *"correct" must be defined before it can be verified*. The Liskov Substitution Principle, articulated with Jeannette Wing in 1994, made the connection precise: a program's correctness is a property of its *specification*, not just of its execution.[^1] The chapter's three-pass verification is Liskov's principle applied to terminal builds. Pass 1 (mechanical) checks execution. Pass 2 (scope) checks against the spec. Pass 3 (intent) checks against the formulation. The hierarchy — execution → specification → intent — is the hierarchy Liskov's framework formalizes for software more broadly. The post-build learning document, in turn, is the practitioner's record of where the hierarchy held and where it did not, which is the cognitive event that makes the next build's hierarchy tighter.

---

## Bridge

You have the discipline. Chapter 14 hands you the build.

---

[^1]: Liskov, B. and Wing, J. M. "A Behavioral Notion of Subtyping." *ACM Transactions on Programming Languages and Systems* 16, no. 6 (1994): 1811–1841.
