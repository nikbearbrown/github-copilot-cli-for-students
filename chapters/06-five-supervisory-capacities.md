# Chapter 5 — The Five Supervisory Capacities

> These are the five things you do that `gh copilot suggest` cannot. Name them. Practice them. Never delegate them.

---

## Learning outcomes

1. **(Remember)** Name and define the five supervisory capacities.
2. **(Apply)** Identify which supervisory capacity is being exercised at each step of a provided terminal build sequence.
3. **(Analyze)** Diagnose a build that went wrong by identifying which supervisory capacity was absent.

---

## Opening

Seth was mid-build on a Git workflow script. The CLI had generated a `git filter-repo` invocation to remove a leaked API key from his commit history. The explain step had confirmed what the command would do. He almost ran it.

He stopped.

He could not point to a specific thing wrong with the command. The syntax was right. The explanation made sense. But something — a feeling, not a thought yet — said *don't*. He sat with the feeling for a moment. Then he could articulate it: the filter-repo would rewrite the entire branch history, and he was about to do that on a branch that he had already pushed to a shared remote, where two collaborators had local clones. The rewrite would invalidate their clones. They would arrive at work tomorrow to find their git in a state they could not push from.

The feeling has a name. It is called **plausibility auditing** — the supervisory capacity of *hearing the wrong note before verification catches it*. The capacity is one of five that this chapter names. Together, the five capacities are the work the conducting discipline keeps yours.

<!-- → [DIAGRAM: Five supervisory capacities as a five-column layout. Each: abbreviation, plain name, terminal-specific one-sentence definition.] -->

---

## Why name them

You already do supervisory work, even without the framework. Every time you stop before running a command and think "wait" — that is supervisory work. Every time you reword a `gh copilot suggest` prompt because the first one was vague — that is supervisory work. Every time you decide to write a script by hand because the operation is too consequential to delegate — that is supervisory work.

The point of the framework is not to introduce something you do not do. It is to *name* what you already do, so you can deploy it deliberately and so you can diagnose what went wrong when something breaks.

A build that goes wrong is not a mystery. It is one of five named failures. The post-mortem becomes structured: *which capacity was absent at which step?* Naming makes recovery operational.

The five capacities have two-letter abbreviations because you will use them in build logs. The book uses the same five across the series; if you read *Codex for Students* or *Claude Code for Students*, you will recognize them.

---

## [PA] Plausibility Auditing

**The capacity to hear the wrong note in a command explanation before running it.**

The feeling Seth had with the `git filter-repo` was PA. The output was fluent. The handoff conditions he had written would have passed. Something matched what he knew about his situation poorly enough that he stopped before running.

Plausibility auditing is the capacity that catches the dangerous middle. The CLI generates output calibrated for *accuracy against the patterns it learned*. The accuracy is real but partial. The match against *your specific situation* is yours to check. PA is what fires when the fluent output and your knowledge of the situation diverge.

Terminal example:

> The CLI explains: *"This `rm -rf` removes all files matching the pattern in the specified directory."*
>
> The explanation is correct. PA fires: the pattern Seth specified is `*.bak`, but the project contains an `.archive/old-versions.bak/` directory that has files Seth needs. The `rm -rf` will descend into the directory. The CLI did not know.

PA requires that you have domain knowledge of the situation. The more you have done with shell commands and project layouts and Git, the more reliable your PA becomes. It is cumulative. The student who has been at the terminal for two years has weaker PA than the same student in three years. The discipline of *attending to* PA — pausing when you hear the note, investigating rather than dismissing — is what builds the capacity over time.

---

## [PF] Problem Formulation

**The capacity to decide what the build IS before `gh copilot suggest` sees it.**

The CLI optimizes within the frame you give it. If the frame is wrong, the output is wrong, elegantly.

Terminal example:

> Seth needs to "clean up his Downloads folder." This is the frame.
>
> The CLI, given that prompt, will generate a `find` command that matches some files for deletion. The frame is too vague to produce the right command.
>
> Seth reformulates: *"Move files in `~/Downloads/` that are over 100 MB AND have not been opened in the last 30 days AND are not in `~/Downloads/dont-touch/` to `~/archive/Downloads/`."*
>
> Now the CLI can produce a useful command. The reformulation is the work that mattered.

PF is the work *before* the suggest. Use `gh copilot ask` to interrogate the problem space — "what would I need to know about my Downloads folder to write a cleanup script?" — and let the interrogation surface frames you had not considered. The deciding is yours; the interrogation is help.

PF is the most under-exercised capacity. The pressure to *get going* is what skips it. The student who pauses to formulate properly produces builds that work; the student who jumps to suggest produces builds that the rest of the framework spends time recovering from.

---

## [TO] Tool Orchestration

**The capacity to choose which of the three CLI tools in what order, with what context.**

You have three `gh copilot` tools (Chapter 4). You have your text editor. You have your shell history. You have a notebook (the CLI.md from Chapter 6). You have `gh copilot` plus the option of *not* using `gh copilot* and writing the command yourself. TO is the scheduling work.

Terminal example:

> Seth needs to write a `git pre-commit` hook that runs lint and tests.
>
> TO question 1: Is this a one-shot or a recurring task? Recurring. So the hook needs to be a file in `.git/hooks/`, not a one-time `git commit` decoration.
>
> TO question 2: Does Seth know the syntax for pre-commit hooks? Roughly. So `gh copilot ask` is the right tool for the *concept* check, and `gh copilot suggest` is the right tool for the *script generation*.
>
> TO question 3: Should Seth write the hook by hand instead? The hook will run on every commit; the cost of an error is high. So even though `gh copilot suggest` could generate it faster, Seth chooses to write it by hand and use `gh copilot explain` on each component as he composes.

TO is the conducting metaphor's most literal expression — you are scheduling the instruments. The decision of *which instrument to use, when, and with what context* is the work.

---

## [IJ] Interpretive Judgment

**The capacity to supply meaning the CLI's explanation cannot supply.**

The explanation tells you what the command *does*. IJ tells you what the command *means* — in your project, for your goal, given the consequence horizon you care about.

Terminal example:

> The CLI explains: *"This `find` produces a list of 47 files matching the pattern."*
>
> The explanation is correct. IJ fires: 47 files is *a lot* for what Seth was expecting; the project has been running about a month and the natural rate of file generation is about one file per day, so 47 is high. Seth investigates and finds that a build script is producing duplicate logs. The 47 is the symptom; the duplication is the cause.

IJ overlaps with PA but is distinct. PA fires *defensively* — something is off. IJ fires *constructively* — here is what would actually serve. PA notices the mismatch; IJ supplies the interpretation. In practice, they often run together: PA notices, IJ explains.

The capacity that requires the most domain knowledge. It is also the capacity that develops fastest with deliberate practice — every time you stop to interpret an output, you are exercising IJ.

---

## [EI] Executive Integration

**The capacity to hold the whole build toward a single goal across many commands.**

A long terminal session has many commands. Each is locally reasonable. Without EI, you finish the session and find that the cumulative result violates a constraint you set thirty commands ago.

Terminal example:

> Seth's CLI.md (Chapter 6) for a project includes the rule "never push directly to `main`." Two hours into a build session, Seth runs:
>
> `gh copilot suggest "push my changes"`
>
> The CLI returns `git push origin main`. The push is the natural completion of the local commits Seth has been making. The CLI did not know about the rule.
>
> EI fires: the CLI's suggestion violates the project rule. Seth catches it. He revises: pushes to a feature branch, opens a PR.

EI is the capacity that catches drift. It requires holding the project's constraints across the duration of the session. It is exercised across sessions too, with CLI.md as the persistent record — the constraint that the CLI cannot remember between sessions is the constraint EI will catch this session, *if* the constraint is in CLI.md.

EI is the integration check. It is what makes the build coherent.

---

## The capacities in motion: a worked sequence

A Git automation build, with capacities labeled at each step.

| Step | Action | Capacity |
|------|--------|----------|
| 1 | Decide the build is a recurring hook, not a one-shot | **PF** |
| 2 | Open `gh copilot ask` to check pre-commit hook conventions | **TO** |
| 3 | Read the answer; notice it does not address my use of lint vs. tests order | **PA** |
| 4 | Ask follow-up about order; learn that lint-first is conventional but not required | **IJ** |
| 5 | Decide tests-first for my project (failures should be fast) | **PF** revisit |
| 6 | `gh copilot suggest "pre-commit hook that runs tests then lint, fails fast"` | **TO** |
| 7 | Read the generated script; notice it uses `set -e` which I need to understand | (informational) |
| 8 | `gh copilot explain "set -e in a bash script"` | **TO** |
| 9 | Read the explanation; predict (correctly) that set -e exits on first failure | (informational) |
| 10 | Notice the script's lint step would also test files outside the staged changes; this is wrong for a pre-commit hook | **PA** + **IJ** |
| 11 | Reformulate: hook should only run lint on staged files | **PF** revisit |
| 12 | New suggest with the staged-only constraint | **TO** |
| 13 | Read the revised explanation; verify staged-only behavior | **PA** |
| 14 | Place the hook in `.git/hooks/pre-commit`, chmod +x | **TO** |
| 15 | Test by making a small commit; verify the hook runs and fails fast on a test failure | (verification step from Chapter 4 gate) |
| 16 | Update CLI.md: rule added that all hooks should operate on staged files only | **EI** |

Sixteen steps. All five capacities fired. The build is a working hook. The CLI.md is updated. The next hook Seth writes will start from a better place.

The discipline is naming the capacities as they fire, in build logs you keep for yourself. The naming makes the work visible, deployable, teachable.

---

## Diagnosing a build that went wrong

The capacities are diagnostic. When a build produces a bad outcome, ask: *which capacity was absent?*

- **Output is fluent but wrong, no warning signs.** PA was absent (or fired and was overridden).
- **The whole build solved the wrong problem.** PF was absent at step zero.
- **Wrong tool at the wrong time** (e.g., used suggest when ask would have helped, or used the CLI when writing by hand would have been better). TO was absent.
- **Output is technically correct but the file count / path / scope is off for context.** IJ was absent.
- **Final result violates a project rule set earlier in the session.** EI was absent.

The framework is most useful in post-mortems. When a build breaks, the question is not "what should I have done?" — it is "which of the five was absent, where, and what would the build look like if it had fired?" The structured question makes the next build better.

---

## Common misconceptions

**"I have to consciously run through all five every prompt."** No. Most prompts exercise one or two. PF dominates at the start of a build. TO fires at tool-choice decisions. PA fires on output review. IJ fires on revision. EI fires when drift threatens.

**"With practice they fuse into a single 'judgment'."** Yes — and you should let that happen. The named decomposition is for the period when you are still learning to recognize what you are doing. Later, the names recede; the practice remains.

**"PA = paranoia."** No. Vigilance, not anxiety. The discipline of *attending to* the off-feeling, not the practice of suspecting all output.

**"These are abstract; I want concrete rules."** The five are the abstraction *under* the concrete rules. The concrete rules (run explain on every suggest; use --dry-run for destructive commands; check the file count before `rm`) are how the capacities show up at the surface. Knowing the capacities lets you write new concrete rules for situations the book did not anticipate.

**"The capacities will be obsolete when the CLI gets better."** Possibly, in domains the CLI can fully reach. The capacities that require knowledge the CLI does not have (plausibility against domain knowledge, problem formulation against intent, interpretive judgment against project context, executive integration across persistent constraints) will remain irreducibly yours for as long as the CLI does not have access to your filesystem, your goals, and your history. None of these are on the immediate horizon.

---

## Exercises

1. **(Apply)** Take your most recent significant terminal session (10+ commands, AI-assisted). Label the supervisory capacity exercised at each step. Honest about cases where no capacity fired.

2. **(Analyze)** Read the following provided CLI transcript where one capacity is systematically absent. Identify which one, name three places where it should have fired, and describe what would have happened differently.

3. **(Create)** Write your own "capacity checklist" for a current shell project — five short prompts, one per capacity, that you can run through your head before consequential commands.

---

## What would change my mind

The chapter's central structural claim is that **the five capacities are not closable by improvements to the CLI itself** — that the supervisory work they describe requires knowledge the CLI structurally does not have. If a 2027 or later CLI demonstrated reliable performance of *all* five capacities on its own — including PA on its own output (which would require some form of meta-cognition the current generation does not have) and EI across multiple sessions (which would require persistent context the current generation does not have) — the chapter's claim softens.

I do not expect this on the next-edition timeline. The structural arguments are about *what information the CLI has access to*, and the gap is not narrowing on the immediate horizon. The book operates on the working assumption that the capacities are yours.

---

## Still puzzling

- **The exact boundary between PA and IJ.** Both involve interpretation against domain knowledge. The book's distinction (PA defensive, IJ constructive) is operational but the line is fuzzy in practice.

- **Whether the framework's five-capacity decomposition is the right number.** Other frameworks use three (judgment / verification / integration) or seven. The book's five is the synthesis that has been useful in practice; whether it is *correct* is not a question with a clean answer.

- **Whether the capacities transfer between agentic-tool surfaces.** The book teaches them for `gh copilot suggest`. The companion books (Claude Code, Codex) teach the same five. Whether the capacities a student practices in one surface develop into the same capacities for another is an empirical claim the book makes but does not directly measure.

---

## AI Wayback Machine

🕰️ **Douglas Engelbart** (1925–2013) — engineer whose 1962 SRI report *Augmenting Human Intellect: A Conceptual Framework* established the modern intellectual project of computer-human partnership. Engelbart argued that the right question about computer technology was not *can it replace the human?* but *can it extend what the human can think and do?* His framework identified the cognitive capacities that augmentation should target and the capacities it should leave to the human.[^1] The five supervisory capacities in this chapter are Engelbart's framework applied to AI-assisted terminal work — a precise specification of which cognitive work the tool extends (pattern completion) and which it does not (plausibility auditing, problem formulation, tool orchestration, interpretive judgment, executive integration). Engelbart never wrote about `gh copilot suggest`. His framework anticipated it.

---

## Bridge

You have the capacities. Chapter 6 introduces CLI.md — the file that makes the EI capacity scale across sessions and that gives the discipline a persistent home.

---

[^1]: Engelbart, D. C. *Augmenting Human Intellect: A Conceptual Framework*. SRI Project No. 3578 (Air Force Office of Scientific Research), 1962. Available via the Doug Engelbart Institute.
