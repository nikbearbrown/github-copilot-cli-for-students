# Chapter 14 — Your First Full Build: From Problem to Verified Output

> You have the discipline. Here is the project. Conduct it.

---

## Learning outcomes

1. **(Create)** Plan, execute, and verify a student-scale shell project using the complete conducting framework.
2. **(Evaluate)** Assess your own build against the five supervisory capacities.
3. **(Create)** Produce a post-build document.

---

## Opening

Not Seth's build.

Yours.

The chapter gives you the project brief, the tools, and the sequence. Everything else is your decision. By the end of the chapter, you will have shipped one fully conducted shell project — planned with a problem formulation and CLI.md, executed with the suggest → explain → verify gate, verified against intent, documented in a post-build learning record.

The chapter is short. Most of it is the brief. The rest is the discipline you have been practicing for thirteen chapters, applied without scaffolding.

---

## The project brief

A shell project at student-scale that exercises all five supervisory capacities.

**Recommended project: a repository hygiene script.** The same project Seth used in Chapter 11–13 — extend it, harden it, or adapt it to your stack. The brief:

> Build a shell script that reduces disk usage in a git repository by removing *generated artifacts* (`.godot/imported/`, `.import/`, `addons/.cache/`, `builds/`, or the equivalent for whatever stack you work in — `node_modules/`, `dist/`, `target/`) from non-active branches, while preserving the currently-checked-out branch and any explicitly-preserved reference snapshots.
>
> Inputs: the git repository to clean (default: current directory).
> Outputs: a report of branches cleaned and disk space recovered, written to stdout. Optionally: a log file at `~/.repo-hygiene-history.log` with timestamps.
> Constraints: the active branch must be unchanged after the script runs. The script must not delete files that are not generated artifacts. The script must not modify `.git/objects/` or other git internals.

If the repository-hygiene project does not match your situation, five alternatives — pick the one that maps to a project you actually own:

**Alternative A: a Godot export-preset validator.** Build a script that reads `export_presets.cfg` for one of your Godot projects and verifies that each preset has the right signing config, icon sizes, version string, and feature tags before you publish a build. Catches the "I forgot to bump the Android version code" failure that costs you a Play Store submission.

**Alternative B: a Roblox place-file packaging script.** Build a script that takes the latest `.rbxlx` of a Roblox project (e.g., Midnight Fuel), zips it together with the asset folder, the changelog, and a generated release-notes file, and writes a release package to `~/releases/<project>-<date>.zip`.

**Alternative C: a weekly dev-log generator.** The creative-build from Chapter 11 — reads git commits across your active repos plus a drafts folder plus a to-do file, produces a markdown weekly dev log in your voice. Apply the *Voice and aesthetic conventions* discipline you wrote in Chapter 11's CLI.md section.

**Alternative D: a custom `CLI.md` / `AGENTS.md` / `CLAUDE.md` for one of your own projects.** Author the persistent-context file at full strength for a project you maintain. Use Walker and Zelda (Appendix A — `chapters/98-appendix-walker-and-zelda.md`) as reference implementations of what a mature CLI-discipline document looks like. The "build" here is the document; the verification is whether a fresh CLI session that reads it produces work consistent with your standards.

**Alternative E: a daily-backup script or a development-environment bootstrapper.** Back up your `~/Projects/` to a specified location with per-project archives and a restore-test; or bootstrap your dev environment on a new machine idempotently (dotfiles, CLI tools, git config, Godot/Roblox/Node toolchains).

All projects exercise all five capacities. All are deployable in your own work. All produce a post-build document worth writing.

Pick one. Or pick something else of comparable shape and scope (multi-step, touches the filesystem or a persistent-context file, needs handoff conditions, has a dangerous-middle hazard). The brief is the orientation; the choice is yours.

---

## The complete sequence

The book in fourteen steps. Apply each one to your project.

1. **`gh copilot ask`** to interrogate the problem space (Chapter 7).
2. **Problem formulation** — three one-sentence answers (Chapter 7).
3. **CLI.md** — four sections, under 200 lines (Chapter 6).
4. **Execution plan** — one page with handoff conditions per step (Chapter 11).
5. **Plan review** — read critically, identify dangerous-middle hazards (Chapter 11).
6. **First `gh copilot suggest`** — with five-element specification (Chapter 8).
7. **`gh copilot explain`** — predict, then read (Chapter 4).
8. **Verify in dry-run** — `--dry-run`, `echo`, `-print` (Chapter 4).
9. **Execute** — only after verification (Chapter 4).
10. **Handoff condition check** — specific, testable, binary (Chapter 9).
11. **Plausibility audit on the result** — trust the off-feeling (Chapter 5's PA).
12. **Update CLI.md** with any lessons learned (Chapter 6).
13. **After all steps: three-pass verification** — mechanical, scope, intent (Chapter 13).
14. **Post-build learning document** — five sections, one page (Chapter 13).

The sequence is the conducting discipline at full operational form. Steps 6–12 repeat per step in the build. Steps 1–5 happen once, upstream. Steps 13–14 happen once, at the end.

---

## What success looks like

Not a perfect build.

A build where:

- You can **account for every command** that ran. What it did, why you ran it, what alternative you considered, what you would change.
- The **CLI.md grew** with lessons learned from this build. Even one entry counts.
- The **post-build document** contains a specific "what I would do differently" that names a real decision you would reverse.
- The supervisory work was **labeled** in the build log — at least the major capacity moments (PA catches, PF revisits, EI integration checks).
- The **project works** for the use you built it for. (This is necessary but secondary; the discipline is the point.)

A build that meets these criteria is the chapter's success. Whether it took two hours or six is less important than whether it taught you what the discipline costs and what it provides.

---

## What it will probably *not* be like

A few things to expect.

**The first attempt will produce at least one failure that the discipline catches.** A handoff condition that fails on a first run. A plausibility audit that fires on something the spec did not anticipate. A revert-and-respecify on a step where the second correction did not work. These are not signs of a failed build; they are signs of the discipline working.

**The build will take longer than you estimate.** The first conducted build always does, because the upstream formulation work is not yet habitual and the per-step gate feels heavy. The second conducted build is faster. The third is materially faster. By the tenth, the discipline is reflex and the upstream investment is invisible.

**You will be tempted to skip the post-build document.** "I'll write it tomorrow." You will not write it tomorrow. Write it now, at the end of the build, before the lessons fade. Thirty minutes. One page. The artifact pays back.

**The CLI.md you write will be revised twice during the build.** The first version will be incomplete; the build will surface things you didn't know to write down. This is fine. Revise as you go. The version at the end of the build is the version that records what you actually learned.

---

## What you have

By the end of this chapter, you have:

- A **working shell project** that does something useful in your own work.
- A **CLI.md** for the project, with lessons-learned entries from this build.
- A **build log** noting where the supervisory capacities fired.
- A **post-build learning document** — five sections, honest — that consolidates what you learned.
- A **practice** of the conducting discipline that the next build will be faster because of.

You do not have a finished discipline. You have the beginning of one. The framework — gate, capacities, CLI.md, handoff conditions, dangerous middle, three-pass verification, post-build document — is now something you have *done*, not just read. The next time you face a terminal task, you will know what conducting it would look like.

That is what the book has been pointing at.

---

## Closing

You built it. You can explain every command that ran. You know what every flag does. You know why every scope decision was made. You know what you would do differently.

That is what the terminal feels like when you are the conductor. That is what `gh copilot suggest` is for.

The next build starts here.

---

## Exercises

1. **(Create)** Complete your full conducted build, end to end. Submit the post-build learning document alongside the working script. The document is the primary artifact; the script is verification that the discipline produced something.

2. **(Evaluate)** Return to the paragraph you wrote in Chapter 0 Exercise 2 — the calculator-and-arithmetic paragraph. Rewrite it now, with the build behind you. What changed?

3. **(Evaluate)** Which of the five supervisory capacities was hardest to exercise consistently in your build? Design a practice exercise targeting that specific capacity for your next build.

---

## What would change my mind

The chapter's claim is that **completing the conducted build produces a step-change in supervisory capacity** — that the practitioner who has built one full project with the discipline is materially more reliable on subsequent terminal AI use than the practitioner who has only read about the discipline. If a controlled comparison — students who completed Chapter 14 vs. students who completed Chapters 0–13 without the capstone — found no measurable difference in subsequent build quality, the chapter becomes formative rather than load-bearing. The book would still recommend it; the case for "the build is the lesson" weakens.

I expect the difference to be substantial because the capstone is where the framework becomes practice. Reading produces the framework; building consolidates it.

---

## Still puzzling

- **The right scope for the capstone build.** The book recommends a repository-hygiene script. Some students will want something more ambitious; some, less. Where the right size is varies by reader.

- **Whether the post-build document should be shared with a peer.** Sharing produces accountability for honesty (the section "what I would do differently" gets fuller under peer review). It also produces the incentive to perform. The book's stance: write candidly for yourself first, then optionally share a redacted version.

- **What comes after the chapter.** Most readers will do a second build, then a third, and the discipline will become reflex over months. Some will return to *Codex for Students* or *Claude Code for Students* to learn the editor variants. Some will reach for the teachers companion to start designing classroom activities. The book ends; the practice continues.

---

## AI Wayback Machine

🕰️ **John Dewey** (1859–1952) — philosopher of education whose *Experience and Education* (1938) argued that learning is *the transformation of the learner through purposeful experience*, not the deposit of information into the learner. Dewey's claim was that the cognitive structures that constitute durable learning are built through *engaged practice* on real tasks, not through reading-about-practice or being-told-about-practice.[^1] The post-build learning document is Dewey applied to terminal AI: the record that the experience changed the person, not just the repository. The full conducted build is the experience. The document is the record of what the experience built. Dewey was writing about classroom learning; the form scales to the practitioner's first capstone build.

The book ends here.

You are now the practitioner.

The next build is yours.

---

[^1]: Dewey, J. *Experience and Education*. Macmillan, 1938. The Kappa Delta Pi reprint (1998) is the standard recent edition.
