# Chapter 8 — Writing `gh copilot suggest` Prompts That Are Specifications

> "Archive log files" is not a prompt. A prompt names the files, the destination, the exclusions, and what must not be touched.

---

## Learning outcomes

1. **(Understand)** Distinguish a prompt (request) from a specification (complete task definition).
2. **(Apply)** Rewrite a weak `gh copilot suggest` prompt as a complete specification using the five-element format.
3. **(Analyze)** Identify what is missing from a set of provided prompts that would cause the CLI to produce incorrect output.

---

## Opening

The chapter is short. The discipline is one paragraph; the rest is the practice.

You have a problem formulation (Chapter 7). You have a CLI.md with the project's rules (Chapter 6). You are about to type a `gh copilot suggest` prompt. The discipline is to write the prompt as a **specification** — five elements, each specific enough that the CLI cannot reasonably misinterpret.

The five elements:

1. **Operation.** The specific verb. *Move*, not "handle." *Copy*, not "deal with." *Find and list*, not "look at."
2. **Scope.** Which files, which directories, which patterns. With paths.
3. **Exclusions.** What the command must skip. Directory exclusions. File-pattern exclusions. State-based exclusions (held-open, modified-since, gitignored).
4. **Output format.** What the command produces. A list to stdout? A modified file? A new file at a specified path?
5. **Negative constraint.** What the command must *not* do. The "do not delete." The "do not modify in place." The "do not run if condition X is unset."

Same task, two prompts:

> *"Archive log files."* (Request.)
>
> *"Move all files matching `*.log` under `~/projects/my-project/logs/` that are not currently held open by any process, to `~/archive/logs-2026/`. Skip any `.log` files in `~/projects/my-project/.git/` or `~/projects/my-project/node_modules/`. Print the count of files moved. Do not delete source files; do not modify any file outside `~/archive/logs-2026/`; do not run if `~/archive/logs-2026/` does not already exist."* (Specification.)

The CLI's output for the request is generic. The CLI's output for the specification fits the project. Same CLI. The five elements are the difference.

<!-- → [TABLE: Prompt vs. specification — two columns, five rows. Each row: one element. Left: weak prompt version. Right: specification version. Applied to terminal tasks.] -->

---

## Why the elements matter

Each element protects against a class of failure.

**Operation specificity** protects against the CLI choosing a different verb than you meant. "Handle the old files" can become `mv` or `rm` or `cp` — the CLI picks one. If the wrong pick is destructive, the silent failure is severe. Specifying the operation forces the CLI to your verb.

**Scope specificity** protects against the CLI's glob being wider or narrower than you meant. "Log files" can match `*.log`, `*.log.gz`, `*.LOG`, `*.log.[0-9]*`, depending on what the CLI's training data weighted. Specifying the pattern eliminates the ambiguity.

**Exclusion specificity** protects against the dangerous middle from Chapter 2. The `find` that descends into `.git/` because no exclusion was named is the canonical case. Naming exclusions is the operational form of the scope-judgment work the CLI cannot do.

**Output-format specificity** protects against the CLI producing a different shape than you can consume. A command that produces a list when you wanted a single file, or vice versa, is wrong even if every other element is right. The output format is the handoff to whatever comes next.

**Negative-constraint specificity** is the most under-used element and the one that catches the dangerous middle most often. *"Do not delete"* is one negative constraint. *"Do not run if `$VAR` is unset"* is another (the Steam Linux `rm -rf $STEAMROOT/` bug from 2015 was the absence of this constraint).[^1] *"Do not modify any file outside the target directory"* is a third. Writing negative constraints forces you to think about what the command must not do, which is where the surprises live.

---

## Worked example: the five elements applied

The class-website deploy script's first build step.

**Operation:** Copy.

**Scope:** Files under `~/projects/class-website/src/`, plus assets under `~/projects/class-website/assets/`.

**Exclusions:** Skip `*.bak` files. Skip `node_modules/` directories. Skip `.git/`. Skip any file matching `*.private`.

**Output format:** Files written to `~/projects/class-website/dist/` with the same relative directory structure as source. A manifest file written to `~/projects/class-website/dist/MANIFEST.txt` listing what was copied.

**Negative constraint:** Do not modify source files. Do not write outside `~/projects/class-website/dist/`. Do not delete files already in `dist/` that no longer exist in source — that is a separate cleanup step. Do not push to the school server — that is a separate deploy step.

The specification is roughly 100 words. The CLI's resulting suggest is targeted. The `gh copilot explain` step confirms the command does what the specification asked. The build proceeds.

A weaker prompt — "copy source to dist" — would have produced a `cp -r` that descended into `node_modules/` and copied gigabytes that did not need copying. Same CLI. The 100 words of specification are why one outcome happens and the other does not.

---

## Common misconceptions

**"Specifications are for big builds."** No. Any operation that can fail silently deserves the five-element format. The cost of writing 100 words is small. The cost of catching a silent failure with a vague prompt is large.

**"More words = better."** No. Precision per element, not verbosity. A 30-word specification with each element specific is better than a 200-word one with vague elements. The test is per-element specificity, not length.

**"I can just edit the generated command."** Sometimes. The dangerous middle is when the generated command is not visibly wrong. Specification prevents the dangerous middle by giving the CLI no room to interpret. Editing-after-the-fact only catches the cases where the wrongness is visible.

**"Negative constraints feel paranoid."** They are not paranoia. They are the operational form of the lessons learned in CLI.md and of the "never" rules the project enforces. Naming them in the prompt is the protection against the CLI producing output that violates them.

**"The CLI doesn't need all five; it'll figure out the rest."** The CLI will fill defaults for missing elements. Defaults are averages across the training data; you are not the average. The cost of relying on defaults is the silent failure when the default is wrong for your project.

---

## Exercises

1. **(Apply)** Take three `gh copilot suggest` prompts from your recent history. Rewrite each as a five-element specification. Run both versions through the CLI. Document the differences.

2. **(Analyze)** A provided specification is missing two elements (you will see which two are absent). Identify them and explain what the CLI will do wrong as a result.

3. **(Create)** Write a complete five-element specification for the next step in a build you are working on. Apply the gate from Chapter 4 to the specification (suggest → explain → verify).

---

## What would change my mind

The chapter's strong operational claim is that **the five-element specification format produces materially better `gh copilot suggest` output** than free-form prompts. If a controlled comparison found no measurable difference in output quality, build time, or correction rate on matched tasks, the format becomes a checklist rather than a load-bearing discipline. The chapter would still teach it as a first habit; the case for "every consequential prompt" softens.

The chapter operates on the prompt-engineering literature (which broadly supports specificity) and on the structural argument that explicit specification removes interpretation room the CLI would otherwise fill with averages.

---

## Still puzzling

- **How much of the format can be omitted as the CLI improves?** Frontier-generation models handle vague prompts better than 2023-generation models. Some elements (explicit invariants, explicit negative constraints) may become less essential as model behavior matures. The book's prescription assumes the current state; the right granularity for next-year's models is open.

- **Is there a fast path for trivial commands?** A `ls`. A `git status`. The format feels heavy for these. The book's heuristic: skip the format for commands whose entire task fits in one verifiable sentence. Where the threshold sits is fuzzy.

- **Does the format transfer to non-`gh copilot` agentic tools?** The five elements are framed for `gh copilot suggest`. The same five (operation, invariants/scope, context, output format, negative constraint) work for Codex and Claude Code. Tool-agnostic at the conceptual level; details of phrasing differ.

---

## AI Wayback Machine

🕰️ **Ada Lovelace** (1815–1852) — English mathematician whose *Notes on the Analytical Engine* (1843) contain what is widely recognized as the first computer program: an algorithm for computing Bernoulli numbers, specified as an explicit ordered sequence of operations with dependencies and side effects. Lovelace wrote her program before any machine existed that could run it. She knew the program would be executed by Babbage's Analytical Engine someday and she wrote it as a *complete specification* — every operation named, every dependency stated, every intermediate result accounted for. The form she used was the form the chapter teaches: specification, not request.[^2] The five-element format is Lovelace's discipline applied to AI-generated shell commands. She specified for a machine that did not exist; you specify for a CLI that is faster than you but does not know your situation. The form is the same. The discipline she demonstrated in 1843 — make every decision explicit, leave no interpretation room — is the discipline the chapter operationalizes.

---

## Bridge

You have specifications. Chapter 9 addresses what happens when the specification is right and the output is *still wrong* — the dangerous middle, named.

---

[^1]: The Steam Linux `rm -rf $STEAMROOT/` bug (2015): a shell script with `rm -rf "$STEAMROOT/"*` ran as `rm -rf /*` when `$STEAMROOT` was unset. A negative constraint — "do not run if `$STEAMROOT` is unset" — would have prevented it. Public bug: github.com/ValveSoftware/steam-for-linux/issues/3671.
[^2]: Lovelace, A. *Notes on the Analytical Engine* (1843). Multiple scholarly editions; Note G is the key reference for the Bernoulli-number algorithm.
