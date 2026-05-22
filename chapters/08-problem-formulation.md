# Chapter 7 — Problem Formulation: The Mission Before the Command

> The most expensive mistake in a terminal build happens before the first `gh copilot suggest` invocation. Formulate the problem first.

---

## Learning outcomes

1. **(Understand)** Explain why problem formulation is the most important step in a conducted terminal build.
2. **(Apply)** Use `gh copilot ask` to interrogate a problem before writing a single suggest prompt.
3. **(Analyze)** Identify the sections of a problem brief most likely to reveal a formulation gap.

---

## Opening

Seth set out to "back up his side projects."

He spent twenty minutes composing a `tar` command with `gh copilot suggest`. The command was elaborate — included a date stamp, excluded `node_modules/`, wrote to a specific archive directory, calculated a checksum afterward. He ran it. The backup completed.

Then he tried to *use* the backup, because he had also wanted to test his restore process. He extracted the archive into a scratch directory. He ran one of the projects from the restored copy.

The project was missing its `.env` file. The backup had excluded it (because Seth had told the CLI to "exclude things like `.env` and `node_modules` that shouldn't be in version control"). For most projects, that exclusion was right. For *this* project, the `.env` contained API keys that did not exist anywhere else — they had been rotated last week, and the new values were only in the local file. The backup, faithful to Seth's exclusion rule, had not preserved them. They were gone. Reissuing them would take half a day.

The technical work of the backup was correct. The `tar` flags were right. The exclusion was right by Seth's stated rule. The formulation of the *task* was wrong. Seth had been thinking of his side projects in the abstract — generic projects, generic files-to-back-up, generic exclusions. The right formulation would have been specific to *this* set of projects, and would have included a step that said "for each project, list the files that exist only locally and decide explicitly whether to include them in the archive."

Seth spent thirty minutes on a backup that did not back up the thing that needed backing up. The thirty minutes of suggest-and-tweak were spent because he had not spent the five minutes upstream that would have made the suggest-and-tweak unnecessary.

This chapter is the five minutes upstream.

<!-- → [DIAGRAM: The problem formulation gate — one sentence naming what the script does, what it touches, and what it must never touch. Below the gate: suggest prompts. Above the gate: nothing. Editorial style.] -->

---

## What problem formulation is

Problem formulation is the work of *deciding what the build IS* before you write a prompt the CLI can act on.

For a terminal build, the formulation answers three questions:

1. **What does this script do?** Not "back up my projects" but "create a compressed archive of each project's directory, including all local files, written to a specified backup location with a date stamp."

2. **What does it touch?** Which directories, which files, which patterns. With paths.

3. **What does it never touch?** The exclusions. The grade-book CSV. The Time Machine backup. The `.env` files that contain secrets — *unless* they contain secrets that exist only locally, in which case maybe touch them with care.

If you can answer the three questions in one sentence each — short enough to fit on a notecard — you have formulated the problem. If you cannot, the formulation is not finished and `gh copilot suggest` will not save you.

The chapter's central operational rule:

> **No prompt to `gh copilot suggest` until the formulation passes the one-sentence test on all three questions.**

The rule is short, hard to remember to apply, and the single highest-return discipline in the book. The chapter is how to apply it.

---

## The one-sentence test

For each of the three questions, force yourself to write the answer in one sentence. Short sentences. Specific words.

For the backup-side-projects task that opened the chapter:

- **What does this script do?** *"Create a compressed `.tar.gz` archive of each project directory under `~/projects/`, with a date-stamped filename, written to `~/backups/`."*
- **What does it touch?** *"Each directory immediately under `~/projects/` (one archive per project). Reads files within each directory."*
- **What does it never touch?** *"Does not write outside `~/backups/`. Does not modify any file in `~/projects/`. Does not include `node_modules/` directories within projects."*

Then the question Seth needed to ask but did not:

> *"Are there files in any project that exist **only** locally — that are not in version control and that should be **included** in the backup despite a general exclusion rule? List each one explicitly."*

For Seth's projects, the answer would have included the `.env` files with the newly-rotated API keys. The formulation would have caught it. The thirty minutes of suggest-and-tweak would not have been the problem.

The discipline is to force the questions to specificity. "I want to back up my projects" fails the one-sentence test. "Create a `.tar.gz` of each project directory under `~/projects/`" passes. The difference is the specificity of paths, file types, and exclusions.

---

## `gh copilot ask` as the formulation tool

The CLI has a tool for the formulation step. `gh copilot ask` answers questions about the problem space without generating commands.

You can use `ask` to *interrogate* the problem before you commit to a frame:

```
$ gh copilot ask "what are the considerations for backing up local development projects, especially around .env files and secrets that only exist locally?"
```

The CLI returns a discussion of common patterns, common pitfalls, things to think about. The discussion is not a prompt to act on; it is *material for your formulation*. Read it. Use it. Notice the considerations the CLI surfaces that you had not thought of.

Then formulate. Then suggest.

The pattern: **ask → formulate → suggest → explain → verify**. The first two steps are the formulation work. The last three are the gate from Chapter 4. The five-step sequence is the conducting workflow at full operational form.

You will not run all five steps for every command. A trivial `ls` does not need the formulation interrogation; the suggest-and-explain is fast. The longer, more consequential, less recoverable the build — the more the upstream formulation work pays back.

A heuristic: the formulation work should take *at least one-tenth* of the build time. Five-minute build, thirty seconds of formulation. Hour-long build, six minutes of formulation. Multi-hour build, half an hour. The ratio is not exact; the heuristic is: do *not* skip formulation when the build is long enough to matter.

---

## The minimum viable problem brief

For builds large enough that the one-sentence test feels too compressed, the next-step-up is a four-section brief.

```markdown
# Problem brief — [build name]

## Task statement
What does this build do? One paragraph.

## Scope boundaries
Which files, which directories, which environments. Be specific.

## Exclusions
What this build must NOT touch. Especially the "never" rules from CLI.md
that apply.

## Success definition
What does done look like? What is the check that confirms the build worked?
This becomes the handoff condition (Chapter 9).
```

Four sections. Half a page. Five minutes to write. Pin to a scratch file at the top of your build session.

The brief is the formulation in operational form. Once it exists, the suggest prompts can reference it: *"Per the problem brief: implement step 1 (survey the project directories)."* The brief becomes the context the CLI gets, paste-by-paste, as you compose the build.

---

## What the formulation work is *protecting against*

A useful framing: the formulation step is the only step where mistakes are *cheap*. After you commit to a frame and start suggesting, the cost of revising the frame grows quickly:

- **During formulation:** revising the frame is free. You change the sentence.
- **After the first suggest:** revising means discarding one CLI response. Small cost.
- **After ten suggests:** revising means discarding ten responses and starting over. Medium cost. Also, you have accumulated context bias from the previous frame, which makes the next attempt harder.
- **After running anything:** revising may not be possible. The filesystem has been modified.

The formulation work is *front-loaded discipline that produces back-loaded savings*. The five minutes upstream are repaid by the build that does not need to be redone because the frame was right.

Seth's backup-side-projects example: thirty minutes spent at the wrong frame. Five minutes of formulation would have made the build different. Five minutes invested upstream; thirty minutes plus a half-day of API-key reissuance saved downstream. The ratio is dramatically favorable.

The formulation work is *also* the place where you exercise the **PF capacity** from Chapter 5 most directly. Every time you formulate, you practice. Every time you skip formulation, you don't.

---

## Worked example: a backup script done right

The same task as the chapter opening, formulated properly.

**`gh copilot ask`:** *"What should I consider when backing up local development projects? Specifically: files that exist only locally (like `.env` with secrets); files that I do NOT want to back up (like `node_modules/`); how to handle multiple projects; how to confirm the backup is restorable."*

CLI returns a discussion. Key things Seth had not thought of:
- `.env` files often contain things that are only stored locally; back up unless they truly contain only generic dev secrets that are reproducible.
- `node_modules/` exclusion is right, but lockfile (`package-lock.json`, `yarn.lock`) should *not* be excluded — it determines what `node_modules/` looked like.
- Multiple projects: one archive per project or one archive of `~/projects/`? Per-project is more granular for restore.
- Restorability: extract to a scratch location and try to start the project; if it starts, the backup is valid.

**Formulation (one-sentence answers):**

- *What does it do?* Create a `.tar.gz` per project directory under `~/projects/`, named `<project>-<YYYYMMDD>.tar.gz`, written to `~/backups/`.
- *What does it touch?* Each subdirectory of `~/projects/`. Reads files; does not modify.
- *What does it never touch?* `node_modules/`. `dist/`. `build/`. `.git/objects/pack/` (large; reconstructible from `.git/refs/`).
- *What must be included even when the rule would exclude?* `.env` files (per-project decision; default include unless I review and decide otherwise). Lockfiles. `.gitignore`d files that are not in the exclusion list above.

**Suggest prompts (gate applied to each):**

1. Loop over project directories under `~/projects/`. List each.
2. For each project, generate a `tar` command with the right exclusions and the right archive name.
3. Write the archive to `~/backups/`.
4. After all archives written, generate a script that extracts each archive into a scratch directory and reports whether the project's main file is present.

The build takes longer because each suggest is preceded by formulation work and followed by explain/verify. The result is a backup that includes what Seth needed and a restore test that runs in five minutes.

**The lesson:** the formulation work upstream produces a build whose problems are caught before they cost real time.

**The limit:** formulation does not catch the case where the framing is *right* but a specific implementation detail is wrong. The gate from Chapter 4 (explain + verify) catches those. The discipline is layered: formulation upstream + gate per command. Both, every time.

---

## Common misconceptions

**"I'll figure out the formulation as I go."** Sometimes works. Fails for anything multi-step. The cost of formulating-in-place is high; the saving from formulating-upstream is large.

**"My prompt was clear; the CLI got it wrong."** Usually the prompt was clear *to you* but vague to the CLI. Formulation is the work of writing prompts that are clear from outside your head.

**"Formulation is just over-engineering."** For a one-line `ls`, yes. For anything that touches files irreversibly, no. The ratio (formulation time ≈ 10% of build time) calibrates the level.

**"I can use `gh copilot ask` after the first suggest goes wrong."** You can. The cost is higher. The first suggest has already shaped your context; the ask question now has to recover from the suggest's frame. Ask first.

**"Formulation kills my flow."** The flow it kills is the flow of *guessing*. The flow it produces is the flow of *building things that work the first time*.

---

## Exercises

1. **(Apply)** Use `gh copilot ask` to interrogate a terminal task you are planning to do this week. Take the answer seriously — note one consideration the CLI surfaced that you had not thought of.

2. **(Apply)** Take a recent suggest-driven build that did not go as planned. Reformulate the build using the one-sentence test for the three questions. Compare your reformulation to your original prompt. What changed?

3. **(Analyze)** Read the following minimum-viable problem brief (provided). Identify two sections that are too vague to pass the one-sentence test and rewrite them. What would the CLI do differently with the rewritten brief?

---

## What would change my mind

The chapter's strong operational claim is that **upstream formulation produces materially better build outcomes** than starting from `gh copilot suggest`. If a controlled comparison — same set of multi-step terminal builds, with and without upstream `ask` + formulation — found no measurable difference in outcome quality or build time, the formulation step becomes optional rather than load-bearing. The chapter would still recommend it for the supervisory-practice benefit (PF develops with use); the urgency would drop.

I expect the difference to be substantial on multi-step builds and negligible on one-shot trivial commands. The book's prescription scales with consequence.

---

## Still puzzling

- **The exact threshold below which formulation is overhead.** The 10% heuristic is approximate. Some builds with high stakes warrant more; some with low stakes warrant less. The right threshold per person varies with experience.

- **Whether `gh copilot ask` is the right interrogation tool.** Some practitioners prefer to interrogate the problem in their own head or in a notes file, without involving the CLI. Both are defensible. The book uses `ask` because it surfaces considerations the practitioner might not have generated independently.

- **The relationship between formulation and CLI.md.** A well-maintained CLI.md (Chapter 6) does some of the formulation work — the rules and lessons are already encoded. Whether CLI.md reduces the need for per-build formulation, or whether it complements it, is open. The book's working answer is: complements. Formulation is per-build; CLI.md is per-project. Both, each in their place.

---

## AI Wayback Machine

🕰️ **Frederick Brooks** (1931–2022) — software engineer whose *The Mythical Man-Month* (1975) established that the most expensive bugs in software are bugs of *what was built*, not *how it was built*.[^1] Brooks's argument, refined in his 1986 essay *No Silver Bullet*, was that the cognitive work of *deciding what the system should do* is the irreducible difficulty of software engineering — and no tool eliminates it.[^2] Compilers, debuggers, IDEs, and now AI coding assistants reduce the *accidental* difficulty of software (the typing, the syntax, the wiring). They leave the *essential* difficulty — the formulation work — exactly where Brooks left it: with the human.

The chapter's argument is Brooks applied to terminal AI. `gh copilot suggest` reduces the accidental difficulty of writing shell commands. The essential difficulty — deciding what the command should do, in this project, with this scope, given these exclusions — is yours. Brooks's *Mythical Man-Month* warned that the most expensive bugs come from building the wrong thing. The chapter's formulation discipline is the operational form of building the right thing.

---

## Bridge

You have a problem formulation. Chapter 8 teaches you to write the `gh copilot suggest` prompts that are *specifications*, not requests — the prompts that convert the formulation into commands the CLI can act on.

---

[^1]: Brooks, F. P. *The Mythical Man-Month*. Addison-Wesley, 1975. The 20th-anniversary edition (1995) adds *No Silver Bullet — Refired*.
[^2]: Brooks, F. P. "No Silver Bullet: Essence and Accidents of Software Engineering." *IEEE Computer* 20, no. 4 (1987): 10–19.
