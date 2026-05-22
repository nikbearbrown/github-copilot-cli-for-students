# Chapter 11 — Planning Your First Conducted Build

> Before `gh copilot suggest` runs a single command, you know exactly what you are building, why, and which steps belong to you.

---

## Learning outcomes

1. **(Apply)** Complete a problem formulation and CLI.md for a student-scale shell project using `gh copilot ask`.
2. **(Apply)** Generate an execution plan using `gh copilot ask` and review it before any `gh copilot suggest` invocations.
3. **(Analyze)** Identify the three steps in the build most likely to hit the dangerous middle.

---

## Opening

Seth was planning his first fully conducted build.

He had a `gh copilot ask` open on one side of his screen. He had a notebook open on the other. He had not typed `gh copilot suggest` yet.

This felt wrong. The whole point of using a CLI tool was to *do* things, and Seth was sitting at a terminal with a notebook open, *thinking*. He had been at the planning step for an hour and he had not run a single command. Past-Seth would have started building two suggests ago.

Then he caught it: this was the discipline working. He was not avoiding the build. He was *upstream* of the build, formulating the problem (Chapter 7), writing the CLI.md (Chapter 6), reviewing the plan in his head against the five capacities (Chapter 5). The hour was the upstream investment. The build itself, when it started, would go faster because the upstream investment would have caught the things that would have stopped it.

This chapter is what the hour looks like.

<!-- → [DIAGRAM: The planning sequence — gh copilot ask interrogation → problem formulation → CLI.md populated → execution plan → review and approve → gh copilot suggest invocations. Phase gates labeled.] -->

---

## The planning sequence

The planning phase has six steps. Each takes a few minutes. Together they produce the artifacts the build will execute against.

**1. `gh copilot ask` interrogation.** Use the ask command to investigate the problem space *before* committing to a frame. *"What should I think about when building X?"* The CLI returns considerations. Some you will have thought of; some you will not have. Note the ones you had not.

**2. One-sentence problem formulation** (Chapter 7). Three questions, one sentence each:
   - What does this build do?
   - What does it touch?
   - What does it never touch?

If you cannot answer in one sentence each, the formulation is not finished. Iterate. Use `ask` again on the parts that are fuzzy.

**3. CLI.md populated** (Chapter 6). For a *new* project, write the CLI.md before you start. Four sections — project overview, environment, command conventions, lessons learned (initially empty). Under 200 lines. For an *existing* project, review the existing CLI.md and update if any of the formulation surfaces something new.

**4. Execution plan.** A list of steps, in dependency order, with the handoff condition (Chapter 9) for each. The plan can be in a scratch file, in your notebook, or in a `PLAN.md` you keep alongside CLI.md. The format does not matter; the *act of writing it down* matters.

**5. Review and approve** (your own work). Read the plan. Look for assumptions you have made that you have not verified. Look for steps whose dependencies are not what you assumed. Look for handoff conditions that are weak ("exit 0") and strengthen them.

**6. First `gh copilot suggest` invocation.** Now you start the build. The suggest references the plan; the plan references the CLI.md; the CLI.md references the formulation. Everything is grounded.

The sequence is *front-loaded* discipline. Six steps before you run a single command. The investment is repaid by every command that runs cleanly because the planning caught what would have made it run wrong.

---

## What to interrogate with `gh copilot ask`

`ask` is most useful for the things you do not yet know to ask `suggest` about.

For a new shell build, useful interrogation questions include:

- *"What edge cases should I think about for [the task]?"* — the CLI will surface failure modes you might miss.
- *"What's the typical structure of [the project type]?"* — the CLI's average shape is useful as a starting reference even when your project will deviate.
- *"What are common mistakes when doing [the operation]?"* — the failure-mode catalog the CLI's training has seen most often.
- *"What flags should I consider for [the command]?"* — flag-discovery for commands you have not used in a while.
- *"What's the difference between [option A] and [option B]?"* — disambiguation when you are choosing.

The point is to *learn from the interrogation* — to add to your knowledge before you start building. The discipline of taking the interrogation seriously (reading carefully, noting unfamiliar considerations) is itself an exercise of the PA and IJ capacities from Chapter 5.

---

## What an execution plan looks like

For a student-scale shell project: a list with steps, each step's purpose, and each step's handoff condition. The plan can fit on one page.

Worked example: the plan for a "repository hygiene script" that cleans up generated artifacts (`node_modules/`, `dist/`, `build/`) from non-active branches of a project, preserving the active branch's state.

```markdown
# PLAN — Repository Hygiene Script

## Problem formulation
What does it do? Remove generated artifacts (`node_modules/`, `dist/`, `build/`)
from non-active branches of a git project, freeing disk space without affecting
the currently-checked-out branch.
What does it touch? The non-checked-out branches' worktrees. Reads git state.
What does it never touch? The currently checked-out branch. The `.git/objects`
directory. Anything outside the project root.

## Steps

1. **Survey: list all branches and the disk usage of each.**
   - Tool: `gh copilot suggest` for the `du`/`git for-each-ref` combination.
   - Handoff: a printed list of branches with their disk usage, ordered by size.
     Total accounts for 100% of the project directory's size minus `.git/`.

2. **Identify candidate branches: branches not checked out, last touched > 30 days.**
   - Tool: Custom logic; ask first.
   - Handoff: a list of branch names that meet both criteria. Empty list is a
     valid result (means nothing to clean).

3. **For each candidate branch, switch to it temporarily and delete generated artifacts.**
   - Tool: `gh copilot suggest` for the `git checkout` + `rm -rf` sequence.
   - SUBTLETY: `git checkout` will *fail* if there are uncommitted changes in
     the currently-checked-out branch. Plan must `git stash` before switching.
   - Handoff: each candidate branch has had `node_modules/`, `dist/`, `build/`
     removed. The previously-active branch is restored (checkout back, stash pop).

4. **Verify the active branch is unchanged.**
   - Tool: `git status` + manual check.
   - Handoff: `git status` shows no changes that were not present before the script.
     The current branch matches the branch the script started on.

5. **Print a summary: branches cleaned, space recovered.**
   - Tool: arithmetic from steps 1 and 3.
   - Handoff: the summary is printed to stdout.

## Highest-risk steps (most likely to hit dangerous middle)

- **Step 3** (switching branches and deleting in worktrees). Risks: deleting
  files that are tracked-but-modified on the candidate branch (the user's
  uncommitted-but-stashed work is on the active branch, but a candidate branch
  might have its own uncommitted work from past sessions). Mitigation: check
  each candidate branch for uncommitted changes BEFORE switching; skip the
  branch if any are present.
- **Step 4** (verification). Risks: false-pass if the script left the active
  branch checked-out but the working tree differs subtly (stash pop conflicts).
  Mitigation: explicit diff check, not just branch-name check.

## CLI.md updates this build will require

- Add the project to CLI.md (new project — initialize CLI.md).
- After build: lessons-learned about branch-switching with stash.
```

The plan is roughly one page. It takes ten minutes to write. It references the formulation, the tools, the handoff conditions, the dangerous-middle risks, and the CLI.md implications. It is the artifact the build will execute against.

---

## Reading the plan critically

Before you write the first `gh copilot suggest`, read the plan once more, critically. The capacities from Chapter 5 are the lens.

**PA (plausibility audit on the plan itself).** Does any step feel wrong? Look at the highest-risk steps you identified. Does the mitigation feel sufficient? Trust the feeling — if something is off, investigate before proceeding.

**PF (problem formulation check).** Does the plan address the problem you formulated? Or has it drifted toward an adjacent problem? The formulation in plain English at the top of the plan is the anchor; the steps should serve it.

**TO (tool orchestration).** Is each step's tool choice the right one? Are there steps that should be written by hand instead of generated? Are there steps that should be broken into smaller substeps?

**IJ (interpretive judgment).** Are the handoff conditions specific enough to catch the failures the steps could produce? Or are they "exit 0" in disguise?

**EI (executive integration).** Do the steps, taken together, produce the outcome the formulation specifies? Is there a step missing? A step that violates the formulation's never-touch list?

The pre-build review is the cheap version of the lesson the build will teach you anyway. If you catch a problem here, you save the cost of catching it during execution. If you do not catch it here, you will catch it during execution and the cost will be larger.

---

## The planning gate

The discipline:

> **No `gh copilot suggest` invocation runs until the plan exists, has been reviewed, and the formulation has passed the one-sentence test.**

The planning gate is the chapter's central rule. Like the suggest → explain → verify gate from Chapter 4, the planning gate is non-negotiable for builds long enough to need a plan. For one-shot commands (a `ls`, a `git status`), the gate is overhead. For multi-step builds, the gate is the protection.

The threshold for "long enough to need a plan" is fuzzy. The book's working heuristic: any build whose execution would take more than ten minutes from first command to last verification. Below the threshold, the formulation-and-gate from Chapter 4 is enough. Above the threshold, you need the plan.

---

## What happens during the build

Spoiler for Chapter 12: the build executes against the plan, step by step. Each step gets the full Chapter 4 gate (suggest → explain → verify). Each step's output is checked against the handoff condition. If a condition fails, you revert and respecify (Chapter 9's rule). After two failed corrections, you `/clear` and start the step fresh.

The plan is the spine. The gate is per-step. The capacities label the supervisory work. The CLI.md gets updated as lessons-learned accumulate.

By the end of the build, you have:
- A working shell project (assuming no genuine blockers).
- An updated CLI.md with lessons-learned from this build.
- A build log noting which capacities fired at which steps.
- A post-build learning document (Chapter 14) that consolidates what you learned.

The plan made all of this possible. Without the plan, the build would have proceeded in fits-and-starts, each step generating a new question, each question producing context drift, each context drift producing a build that worked once and could not be reproduced.

---

## Common misconceptions

**"Planning is for big builds."** Even small multi-step builds benefit. The threshold is "more than one command in dependency order." If step 2 depends on step 1, you need at least a sentence of plan.

**"The plan should be exhaustive."** The minimum viable plan is the goal. One page; one paragraph per step; one handoff condition per step. Detail in proportion to consequence — high-risk steps deserve more notes; low-risk steps deserve a sentence.

**"I'll plan as I go."** The cost of planning-in-flight is higher than the cost of planning-upstream. Plan-in-flight means you are formulating mid-execution, which mixes work types and produces worse outcomes.

**"`gh copilot ask` will plan it for me."** `ask` can return a plan-shaped response. The discipline is to write *your* plan first, then use `ask`'s response to check yours, not the other way around. The `ask` plan is the average plan; you are not the average.

**"Surrender to ask-mode planning."** Resist. The plan should be your work, informed by `ask`, not the reverse. The supervisory practice depends on the *you* writing the plan.

---

## Exercises

1. **(Apply)** Produce a `gh copilot ask` interrogation, problem formulation, CLI.md, and plan for the project you will build in Chapter 12. The plan should fit on one page.

2. **(Analyze)** Review your plan. Identify the three steps most likely to hit the dangerous middle. For each, write a strong handoff condition. For the highest-risk step, write a STOP-block-style condition that would catch the dangerous-middle failure.

3. **(Evaluate)** Is your plan ready to govern a build? What is the weakest section? Fix it before Chapter 12 begins.

---

## What would change my mind

The chapter's strong operational claim is that **the planning gate produces materially better build outcomes** for multi-step shell projects than starting from `gh copilot suggest`. If a controlled comparison found no measurable difference in build quality or completion time between plan-first and suggest-first approaches, the gate becomes optional. The chapter would still teach the planning sequence as a supervisory practice; the case for "every multi-step build" weakens.

I expect the difference to be substantial because planning catches the dangerous-middle conditions before they cost real time, and because the post-build documentation (Chapter 14) is dramatically easier when the plan exists than when it does not.

---

## Still puzzling

- **The exact threshold above which planning is worth the overhead.** "Ten-minute build" is a working heuristic, not a measured threshold. Right answer varies.

- **Whether the plan should be a separate file or embedded in CLI.md.** The book separates: CLI.md is project-persistent, PLAN.md is build-specific. Some practitioners merge. Either is defensible.

- **How the plan should evolve as the build runs.** The book's working answer: update the plan at the end of the build to reflect what actually happened, then use the updated plan as the basis for the post-build document (Chapter 14). Some practitioners keep the plan static and write the post-mortem separately. Both work.

---

## AI Wayback Machine

🕰️ **Christopher Alexander** (1936–2022) — architect and design theorist whose *Notes on the Synthesis of Form* (1964) argued that *good design begins with a clear statement of the problem* before any solution is attempted. Alexander's project, refined across his career in *A Pattern Language* (1977) and *The Timeless Way of Building* (1979), was that the practitioner's first work is *understanding the problem the design must solve* — and that solutions developed without first understanding the problem produce things that are technically correct and that do not serve the people who inhabit them.[^1] The planning gate in this chapter is Alexander applied to terminal builds. The formulation, the CLI.md, the plan, the review — all of these are the first work, before the building. Alexander's insistence on understanding the problem first is the same discipline the chapter operationalizes. Alexander was writing about buildings; the chapter is about shell scripts. The form is the same.

---

## Bridge

The plan is complete. The CLI.md is ready. The formulation has passed the one-sentence test. Chapter 12 executes it.

---

[^1]: Alexander, C. *Notes on the Synthesis of Form*. Harvard University Press, 1964. See also *A Pattern Language* (Oxford, 1977) and *The Timeless Way of Building* (Oxford, 1979).
