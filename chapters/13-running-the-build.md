# Chapter 12 — Running the Build: CLI Tasks and Human Tasks

> The plan is approved. Now you execute it — one command at a time, with the suggest → explain → verify gate applied at every step.

---

## Learning outcomes

1. **(Apply)** Execute a conducted terminal build sequence, completing CLI tasks and human tasks in dependency order.
2. **(Apply)** Apply each of the five supervisory capacities at the steps requiring them.
3. **(Analyze)** Identify when a build is going off-script and stop before it breaks.

---

## Opening

Seth's repository hygiene build, Phase 1. The plan is open on one side. The terminal is open on the other. The first step is the survey — listing all branches with their disk usage.

He pastes the relevant CLI.md sections into a `gh copilot suggest` prompt. He references the plan: *"Step 1 from PLAN.md: survey all branches and print each with its disk usage, sorted by size."* The CLI returns a one-line command using `git for-each-ref` and `du`. Seth runs `gh copilot explain`. The explanation walks through the pipeline. Seth predicts what the output will look like; the explanation matches the prediction. He runs the command.

The output is the branch list. The disk-usage column is empty for some branches. Seth pauses.

Plausibility audit fires (Chapter 5's PA). The empty-disk-usage column is wrong. Every branch should have a disk usage. The empties mean one of two things: either those branches have not been checked out recently (so the worktree was never populated locally), or there's a bug in the `du` invocation.

Seth investigates. He checks the script. The `du` is reading from the *current* worktree, which only reflects the *checked-out* branch's files. Branches that have not been checked out have no local worktree, hence no `du` output. The script is technically correct — it reports the disk usage of *the current state of the working directory* — but the *interpretation* the column appears to give (disk usage *of each branch*) is wrong.

The script's exit code is 0. The handoff condition Seth wrote ("a printed list of branches with their disk usage") is met. The condition was the wrong condition.

This chapter is what happens during a build when the discipline catches what handoff conditions miss.

---

## The build loop

Each step in the plan goes through the same loop:

1. **Reference the plan.** *"Per Step N: do X with Y conditions."*
2. **Paste relevant CLI.md sections** into the prompt context.
3. **Compose the specification** (the five-element format from Chapter 8).
4. **`gh copilot suggest`** with the specification.
5. **`gh copilot explain`** the output. Read actively; predict before reading.
6. **Verify** in dry-run mode where possible.
7. **Execute.**
8. **Handoff condition check.** If passes, proceed. If fails, revert (Chapter 9's rule).
9. **Plausibility audit on the result.** Even if the handoff passed, does the result make sense given what you know about the project?
10. **Update CLI.md** if a lesson surfaced.

The loop is one or two minutes per step for a smooth build. When the discipline catches something — like the empty-disk-usage column above — the loop pauses and the catch is handled. The catch is the build's value; without it, the silent failure ships.

---

## The three pivotal moments

Every multi-step build produces at least three moments where the discipline matters. Recognize them as they arrive.

### Moment 1: Handoff failure → revert, do not correct forward

A step's output fails its handoff condition. The temptation is to compose a follow-up prompt that fixes the result. The discipline (from Chapter 9) is to *revert and respecify* instead.

The reasoning, again: forward correction pollutes the session context. After two corrections on the same step, `/clear` the session and start the step from a clean context, with a better specification.

The repository-hygiene build above: the empty-disk-usage column is the catch. The fix is not to add a correction prompt ("also include disk usage from each branch's worktree"). The fix is to *revisit the specification* for Step 1 — the original spec did not say "across all branches' worktrees, not just the current checkout," and the CLI's interpretation was reasonable for the under-specified ask. The revision adds the missing constraint. The next attempt has a clean context and a tighter spec.

### Moment 2: Scope creep → log to CLI.md, decline now

The CLI generates an output that does the requested step *and* offers to do something adjacent. The adjacent thing might be useful. It is not the current step.

The discipline: log the suggestion in CLI.md (or in a scratch task list for the build) and decline now. The current step's focus is preserved. The suggestion survives in writing.

The repository-hygiene build, after the survey is fixed: the CLI offers to *also* identify which branches have never been pushed to remote. Useful information. Not in the plan. Logged for a separate phase. Declined now.

The cost of accepting scope creep is that the current step's focus breaks. The cost of declining is zero — the suggestion lives in the scratch list and gets handled later if at all.

### Moment 3: Plausibility audit → trust the feeling

The current step's output passes the handoff condition. Something about it feels off.

The discipline: investigate the feeling. Do not dismiss it because the condition passed. The condition you wrote was the condition you thought to write; the feeling is the condition you did not think to write.

The repository-hygiene build, in Step 3 (deleting generated artifacts from candidate branches): the script reports having cleaned three branches. Seth notices that the report lists branch names he does not recognize. He pauses. The branches are old experiment branches he had forgotten about — and one of them, on inspection, contains an `.import/` cache that he had intentionally kept (it was a reference snapshot for a tutorial article on Zebonastic about Godot's import behavior). The `rm -rf` is about to remove a directory he wanted to keep.

The handoff condition Seth wrote ("each candidate branch has had `.import/` removed") is *technically met* by the script's behavior. The condition he *needed* was "candidate branches do not include any explicitly-preserved reference snapshots." He did not write the condition because he did not know to write it. Plausibility audit caught the wrongness because Seth recognized the branch name; the condition could not catch it because the condition did not know about the preserved snapshot.

Seth aborts the script before it executes the deletion on the preserved-snapshot branch. He adds the branch name to the candidate-skip list (a new entry in CLI.md's "never touch" section). He restarts the build from Step 2 with the skip list applied.

This is the moment the chapter exists for. The plausibility audit, exercised on every step's result, is what catches the dangerous-middle failures that no handoff condition you wrote could have caught.

---

## What human tasks look like in the build

Not every step in the build is a CLI command. Some steps are *human tasks* — work that should not be delegated.

For the repository-hygiene build, human tasks include:

- The plausibility-audit on the survey output (the empty-disk-usage catch).
- The decision to skip a specific branch from the candidate list (the preserved-snapshot catch).
- The verification that the active branch is unchanged after Step 4.
- The interpretation of the disk-space-recovered summary.

The plan should label which steps are CLI tasks and which are human. The build log should label which capacity (PA, PF, TO, IJ, EI) was exercised at each human step. Together, the labels make the supervisory work visible — to you in retrospect, and to anyone else who reads the log.

A build log entry might look like:

```
Step 1 (survey): CLI generated git for-each-ref + du pipeline.
  Handoff: list of branches with disk usage. PASSED.
  PA: noticed empty-disk-usage columns. Investigated. Spec was under-specified.
  Revert and respecify. CLI.md updated.
Step 1b (survey, revised): CLI generated revised pipeline.
  Handoff: list with disk usage from each branch's worktree. PASSED.
  PA passed. Proceeded.
Step 2 (identify candidates): manual logic. Human task.
  Handoff: candidate list with 5 branches. PASSED.
  IJ: skipped two recently-touched branches per intent (active reference).
Step 3 (delete artifacts): CLI generated git checkout + rm sequence.
  Handoff: artifacts removed from each candidate branch. PARTIAL.
  PA: noticed preserved-snapshot branch in candidates. Aborted before deletion.
  Added branch to skip list in CLI.md.
Step 3b: re-ran candidate identification with skip list. PASSED.
Step 4 (verify active branch): manual check. Human task.
  Handoff: git status clean, branch matches start. PASSED.
Step 5 (summary): CLI generated arithmetic. PASSED.
```

The log is the artifact the next build references. It is also the artifact you read at the post-build document stage (Chapter 14) when you write *what I learned that I did not know before*.

---

## After two failed corrections

The discipline's hardest moment is recognizing when to give up on a step and start it fresh.

The signal: you have tried two corrections on the same step. The CLI has had two chances to produce a working result. The result is still wrong, and your context now contains both failed attempts plus your corrections plus the CLI's responses to your corrections.

The next attempt has a polluted context. The CLI is reasoning against the prior failures as if they are part of the desired result. The third attempt is less likely to work than the first one was.

The discipline:

1. **Stop.** Do not write a third correction.
2. **`/clear` or open a new session.** The context is now clean.
3. **Revisit the specification.** Why did the first two attempts fail? Almost always: under-specification or wrong-frame. The fix is upstream of the suggest, not in the suggest itself.
4. **Rewrite the specification** with the failed conditions as explicit negative constraints. The new spec includes what the prior attempts did wrong, so the new attempt cannot reproduce it.
5. **First attempt with the new spec.** Usually works.

The cost of starting fresh feels high. It is consistently lower than the cost of continuing with polluted context. The book's working rule: after two failed corrections, restart. No exceptions.

---

## Common misconceptions

**"The plan should run smoothly once it's written."** A good plan reduces but does not eliminate failures. The dangerous middle is the failure mode plans cannot fully eliminate. The build's value is partly in the plan's correctness and partly in the discipline of catching the cases the plan did not anticipate.

**"If I update CLI.md mid-build I'll lose my place."** Update CLI.md in a scratch file during the build; commit the updates at the end. The cost of mid-build CLI.md edits is real; the cost of *not* updating is larger because the lessons are lost.

**"Plausibility audit is just being paranoid."** PA is the named capacity that catches dangerous-middle failures. It is not paranoia; it is the exercised supervisory capacity. The chapter is the discipline of *attending* to PA when it fires.

**"My build went smoothly; the discipline must be overkill."** Survivor bias. Builds that go smoothly are builds where nothing was hiding. Builds where something *was* hiding — and the discipline caught it — feel less smooth in retrospect but produce better outcomes.

**"I can document the build later."** You will not. The build log written during the build is real; the build log reconstructed after the build is partial. Write during.

---

## Exercises

1. **(Apply)** Execute Phase 1 of your repository-hygiene build (or another build of your choice, planned in Chapter 11). Document each handoff evaluation, each capacity exercised, each plausibility-audit catch.

2. **(Analyze)** At least one step will produce a result that requires revision. Document the failure: what was the handoff condition; what passed it; what was wrong despite the pass.

3. **(Evaluate)** At the end of Phase 1: what would you change in the plan? In the CLI.md? Update both before Phase 2 begins.

---

## What would change my mind

The chapter's strong operational claim is that **the per-step loop (paste, suggest, explain, verify, execute, handoff check, plausibility audit, log) materially reduces silent failures** in multi-step shell builds. If a controlled comparison found no measurable difference in build quality between disciplined and undisciplined execution of the same plan, the loop becomes optional rather than load-bearing. The book would still teach it for the supervisory-practice benefit; the urgency would drop.

I expect the difference to be substantial because the loop's catches (PA on results, scope-creep declines, revert-don't-correct-forward) address the specific failure modes the data shows produce skill atrophy.

---

## Still puzzling

- **The exact number of steps above which the loop is worth the overhead.** Builds with two or three commands can probably proceed with lighter discipline. The book's working answer: any build whose plan has five or more steps benefits from the full loop.

- **Whether the build log should be public or private.** Public build logs serve other practitioners learning the discipline. Private build logs allow more candid plausibility-audit notes. Probably both — a candid private log, a redacted public version.

- **How the build loop changes when the CLI itself has autopilot.** The post-January-2026 `copilot` CLI has plan and autopilot modes that automate parts of the loop. Whether autopilot mode produces equivalent supervisory practice, or whether it erodes the practice by removing the per-step pause, is open. The book's stance: do not use autopilot for student builds where the practice is the point.

---

## AI Wayback Machine

🕰️ **W. Edwards Deming** (1900–1993) — statistician whose **Plan-Do-Check-Act** cycle became the foundation of modern quality management. Deming argued that quality is built into a process through verification at *every step*, not inspected in at the end. The PDCA cycle iterates: plan the change, do the change at small scale, check the result against the plan, act on what the check revealed.[^1]

The per-step loop in this chapter is PDCA at command granularity. Plan: the specification. Do: the suggest-and-execute. Check: the handoff condition and the plausibility audit. Act: revert and respecify (or proceed) based on what the check revealed. Deming wrote about manufacturing in the 1950s; the cycle scales. The shell command is the unit of work; the loop is the unit of quality. Deming's insight — that quality is built in, not inspected in — is the chapter's insight applied to terminal AI.

---

![W. Edwards Deming](../images/w-edwards-deming-efb.png)

*Puppet Art by [Nik Bear Brown](https://www.nikbearbrown.com/).*

## Bridge

The build is done when it passes the handoff conditions. Chapter 13 defines what "done" actually means at the level of the whole build.

---

[^1]: Deming, W. E. *Out of the Crisis*. MIT Press, 1986; reissued 2018. PDCA is articulated across his work; the 1986 book is the standard reference.
