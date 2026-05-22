# copilot-cli-for-students-a-practitioners-guide
## Full TOC Draft — All Phase Outputs Compiled

**Working title:** GitHub Copilot CLI for Students: A Practitioner's Guide
**Series:** Practitioner Guides for the AI Classroom · Bear Brown & Company
**Author:** Nik Bear Brown · bear@bearbrown.co · Bear Brown, LLC
**Co-author:** Seth Brown
**Document:** Full TOC Draft — compiled from all phase outputs
**Version:** 1.0
**Status:** Pre-production — ready for chapter drafting

---

## Document structure

1. Book Concept and Thesis
2. Learner Profile
3. Book Type and Deployment Specification
4. Field Positioning
5. Three-Act Learning Arc
6. Prerequisite Map
7. Build Arc and Terminal Deliverables
8. Chapter-by-Chapter TOC
9. Chapter Anatomy Template
10. Case Study Strategy
11. Hard Topics, Contested Claims, Aging Risk
12. Market Positioning
13. Feature List
14. Out of Scope
15. Adoption Risk Register
16. Open Questions

---

# PART 1 — BOOK CONCEPT AND THESIS

## Book concept summary

> This book teaches **technically fluent high school students how to
> use GitHub Copilot CLI as a Conductor** — directing the terminal
> through real builds while protecting the cognitive struggle that
> builds their own capability — by showing exactly where to draw the
> line between what the CLI executes and what only the student can do,
> through the story of a student who learned that the most dangerous
> moment in terminal work is when a command runs cleanly and silently
> does the wrong thing. It fills the gap left by generic AI literacy
> courses (which treat students as problems to manage) and
> editor-focused AI guides (which never reach the terminal at all).
> It succeeds when the reader can **run a complex shell build with
> GitHub Copilot CLI and finish it knowing more than when they
> started, not less**.

**One-sentence logline:**
The student who learns to conduct the CLI builds faster and thinks
better; the student who runs generated commands without explanation
builds a convincing script and an atrophying mind — and eventually
deletes something they cannot get back.

## Central thesis

"This book argues that GitHub Copilot CLI is not a shortcut but an
instrument — that the student who always explains a generated command
before running it, who maintains a CLI.md that records what the
terminal needs to know, will build real capability, while the student
who runs `gh copilot suggest` output without the `gh copilot explain`
gate will produce working scripts while their own understanding of
the terminal atrophies, and this matters because the terminal
is the one environment where silent failure has immediate, sometimes
irreversible consequences."

## Thesis test

The TOC reflects the thesis at every act:

- ACT ONE: Seth watches his friends generate shell commands they
  cannot explain and run them anyway. A friend's script silently
  processes every file instead of only new ones. The reader feels
  the cost before they need the solution. ✓
- ACT TWO: The conducting framework — CLI.md, the suggest →
  explain → verify gate, the five supervisory capacities, handoff
  conditions for terminal work — the line is drawn operationally,
  command by command. ✓
- ACT THREE: The reader runs their own first fully conducted shell
  build, from problem formulation through verified output. The
  discipline is practiced, not described. ✓

**Thesis test: PASS**

## What makes this book structurally distinctive

The three books in this series share the same spine and discipline.
The dangerous middle differs by tool:

- **Claude Code (editor):** generated code that compiles, passes
  tests, and is semantically wrong
- **Codex (editor):** AI-generated feedback that passes technical
  criteria but fails pedagogically
- **GitHub Copilot CLI (terminal):** a command that runs cleanly,
  exits 0, and silently does the wrong thing

The terminal raises the stakes because failure is often immediate
and irreversible. A wrong `rm` cannot be undone. A `git push
--force` on the wrong branch rewrites history. A `find` that
matches more files than intended processes all of them. The
conducting discipline is not optional at the terminal — it is the
difference between a student who builds things and a student who
breaks things confidently.

## The CLI.md invention

GitHub Copilot CLI (`gh copilot suggest`, `gh copilot explain`,
`gh copilot ask`) does not read a persistent context file
automatically. Unlike CLAUDE.md (Claude Code) or AGENTS.md (Codex),
there is no file-based injection mechanism.

This book introduces **CLI.md** as a student-designed persistent
context artifact — not read automatically by the tool, but
maintained deliberately by the student and used as:

1. **A paste source** — relevant sections copied into `gh copilot
   suggest` prompts as explicit context
2. **A personal constitution** — project conventions, environment
   quirks, "never do X" rules the student has learned from failures
3. **A build log** — what was built, what commands were accepted,
   what failed and why, what the dangerous middle looked like

CLI.md is pedagogically more honest than CLAUDE.md or AGENTS.md
because the student must **actively decide** what context to give
the tool on every invocation. There is no automatic injection.
Supervisory capacity [TO] Tool Orchestration is exercised every
single time.

---

# PART 2 — LEARNER PROFILE

## Primary reader

A technically fluent high school student, 2026. Already uses
AI tools daily. Has used the terminal for basic Git operations.
Watched friends run generated shell commands without understanding
them. Technically fluent, domain-shallow, honest enough about the
gap to want a discipline that closes it.

**Specific person:** Seth Brown — the Cautious Builder. AP
Computer Science student who noticed that his friends were generating
shell commands with `gh copilot suggest` and running them without
the `gh copilot explain` step. One friend's script silently
processed every file in a directory instead of only the new ones.
Nothing broke visibly. The error was invisible until it mattered.
Seth reached out not for a lecture on terminal safety but for a
concrete operational discipline. He is now co-authoring this book
by practicing the discipline as he writes it.

## Prior knowledge assumed

- Basic terminal usage (cd, ls, mkdir, basic Git)
- Some coding experience (AP CS level or equivalent)
- GitHub account and basic repository workflow
- Familiarity with AI chat tools

## Prior knowledge NOT assumed

- GitHub CLI (`gh`) installation or usage
- `gh copilot suggest`, `gh copilot explain`, `gh copilot ask`
- The suggest → explain → verify gate
- CLI.md concept
- The five supervisory capacities
- Shell scripting beyond basic commands
- Any formal software engineering methodology

## Prior misconceptions

1. "If `gh copilot suggest` generates a command that runs without
   errors, it's correct" — exit 0 is not the same as did what
   I meant
2. "The terminal is just a faster way to do what I can do in
   a GUI" — the terminal operates on system state with no undo
3. "I can just look up what a command does after running it" —
   some commands cannot be un-run
4. "AI suggestions are safer than commands I write myself because
   they're generated by a trained model" — the model doesn't know
   your filesystem, your Git history, or your intent

## Motivation type

**Primary:** Intellectual — Seth and his reader have already
noticed the problem and want the repair kit.
**Secondary:** Professional — they want to build real automation,
not just homework assignments.

## Prerequisite map

| Prerequisite | Safe to assume? | If not: where addressed |
|---|---|---|
| Basic terminal (cd, ls, mkdir) | Yes | — |
| Basic Git (add, commit, push) | Yes | — |
| GitHub account | Yes | — |
| GitHub CLI (`gh`) installed | No | Ch 1 installation |
| `gh copilot` extension | No | Ch 1 installation |
| CLI.md concept | No | Ch 6 (CLI.md chapter) |
| suggest → explain → verify gate | No | Ch 4 (conducting chapter) |
| Five supervisory capacities | No | Ch 5 |

---

# PART 3 — BOOK TYPE AND DEPLOYMENT SPECIFICATION

## Book type

**PRIMARY TYPE:** Practitioner handbook with course-textbook
bones. The student reads it cover to cover once, then consults
it chapter by chapter during builds.

## Deployment specification

**Primary adoption context:**
Self-directed high school student finding it independently —
through recommendation, through the $1 Kindle price, through
a teacher who assigned it.

**Secondary adoption context:**
K-12 CS teachers using it alongside
`copilot-cli-for-teachers-a-practitioners-guide` — the teachers
book references this book explicitly.

**Tertiary adoption context:**
University intro CS and DevOps courses addressing terminal
discipline in AI-assisted workflows.

**Price point:** $1 Kindle — designed for assignment without
budget friction.

---

# PART 4 — FIELD POSITIONING

## The positioning statement — consolidated

- Generic AI literacy courses treat students as problems to manage.
  This book treats them as builders to equip.
- Editor-focused AI guides (Claude Code, Codex) never reach the
  terminal. This book lives there.
- No existing book takes a technically fluent student seriously,
  hands them a terminal discipline, and teaches them that exit 0
  is not the same as correct.
- The CLI.md invention is unique to this book — a student-designed
  persistent context artifact that exercises supervisory judgment
  on every invocation.

## What makes the terminal different

The terminal is the one environment where:
- There is no undo buffer
- Errors are often silent (exit 0, wrong result)
- The scope of a command can expand unexpectedly
- History matters (git rebase, rm -rf)

This makes the conducting discipline not just useful but necessary.
A student who conducts editor-based AI builds and skips the
discipline loses quality. A student who skips the terminal
discipline can lose data.

---

# PART 5 — THREE-ACT LEARNING ARC

## The arc statement

This book takes the reader from **technically fluent but
terminally unguarded** to **disciplined terminal conductor** —
first by naming the risk Seth saw in his friends before he had
vocabulary for it, then by building the operational framework
piece by piece through the three CLI tools, then by running a
complete shell project from problem formulation through verified
output using the full discipline.

## The pebble-in-the-pond opening

Chapter 0 gives the reader Seth watching a friend run a `gh
copilot suggest` output without the explain step. The command
exits cleanly. The friend moves on. Three days later: the wrong
files were processed. Chapter 1 gives them the mechanism. The
problem is felt before the framework is named.

## Act One — The Problem (Chapters 0–3)

**Starting state:** The reader is technically fluent but has no
discipline for terminal AI use.
**Inciting question:** "If the command ran without errors, why
didn't it do what I meant?"
**Transition condition:** The reader must feel the cost of
undisciplined terminal AI use and want a concrete alternative.

## Act Two — The Discipline (Chapters 4–10)

**Starting state:** The reader wants the discipline but has no
framework.
**Ending state:** The reader can run a structured shell build
with explicit handoff conditions, naming which cognitive work
they keep and which they delegate to `gh copilot suggest`.
**Hardest moment:** Chapter 9 — the dangerous middle. A command
that `gh copilot explain` describes accurately, that exits 0,
that is still wrong because the scope was not what the student
meant.
**Transition condition:** The reader must be able to state, for
any terminal task, whether it belongs to `gh copilot suggest`
or to them, and why.

## Act Three — The Build (Chapters 11–14)

**Starting state:** The reader has the framework but has not
applied it to a complete real project.
**Terminal deliverable:** The reader's first fully conducted
shell build — planned, executed, verified, documented in a
post-build learning record.
**The arc closes on the reader, not Seth.**

---

# PART 6 — PREREQUISITE MAP

## Prerequisite chain by chapter

| Chapter | Prerequisites | Load-bearing? |
|---|---|---|
| Ch 0 (Introduction) | None | No |
| Ch 1 (The Silent Failure) | None | No |
| Ch 2 (Division of Labor) | Ch 1 risk established | No |
| Ch 3 (The Terminal Gap) | Ch 1 + Ch 2 | No |
| Ch 4 (Conducting) | Ch 1–3 problem understood | Yes |
| Ch 5 (Five Capacities) | Ch 4 conducting metaphor | Yes |
| Ch 6 (CLI.md) | Ch 4–5 framework | Yes |
| Ch 7 (Problem Formulation) | Ch 6 CLI.md concept | Yes |
| Ch 8 (Specifications) | Ch 7 problem formulation | Yes |
| Ch 9 (Handoff Conditions) | Ch 8 specifications | Yes |
| Ch 10 (Creative Builds) | Ch 4–9 full framework | No |
| Ch 11 (Planning) | Ch 4–10 full framework | Yes |
| Ch 12 (Running the Build) | Ch 11 plan complete | Yes |
| Ch 13 (Verification) | Ch 12 build in progress | No |
| Ch 14 (Full Build) | Ch 11–13 full sequence | No |

---

# PART 7 — BUILD ARC AND TERMINAL DELIVERABLES

## The three build tiers

| Tier | Chapters | Build | Terminal artifact |
|---|---|---|---|
| Foundations | 1–3 | None — observation only | Personal terminal audit |
| Framework | 4–10 | Tool-level exercises | CLI.md; labor separation rule |
| Full build | 11–14 | Complete conducted shell project | Post-build learning document |

## The suggest → explain → verify gate

The fundamental workflow gate of this book. The student never
runs a `gh copilot suggest` output without first running
`gh copilot explain` on it. The verify step confirms the
command does what the explanation says in the actual environment.

```
gh copilot suggest "move all log files older than 7 days to archive"
# → generates command
gh copilot explain "find . -name '*.log' -mtime +7 -exec mv {} archive/ \;"
# → explains what it does
# → student verifies scope before running
```

This gate is the CLI equivalent of:
- Claude Code: Explore → Plan → Implement → Commit
- Codex: Ask Mode → Code Mode

## Terminal deliverable specification

**The reader's first fully conducted shell build:**

Required components:
1. Problem formulation (one-sentence, `gh copilot ask` interrogation)
2. CLI.md populated with project context before build begins
3. Every `gh copilot suggest` output explained before running
4. Build execution log (command, explanation, handoff evaluation,
   supervisory capacity label)
5. Dry-run or sandbox verification before production execution
6. Post-build learning document (five sections)

**Post-build learning document — five sections:**
1. What I built (one paragraph, plain language)
2. What I delegated to `gh copilot suggest` and why
3. What I kept for myself and why
4. What I learned that I didn't know before
5. What I would do differently

**Success criterion:** Not a perfect build. A build where the
reader can explain every command that ran.

---

# PART 8 — CHAPTER-BY-CHAPTER TOC

---

## ACT ONE — THE PROBLEM
*Chapters 0–3: From technically fluent to terminally aware*

---

### Chapter 0 — Introduction: The Cautious Builder

**One-line:** Meet Seth. He noticed something his friends didn't —
that exit 0 is not the same as correct.

**Learning outcomes:**
1. (Remember) Name the difference between a command that runs
   without errors and a command that does what you meant.
2. (Understand) Explain why terminal AI use requires a different
   discipline than editor AI use.
3. (Understand) Describe what "conducting" `gh copilot suggest`
   means vs. running its output directly.

**Opening:** Seth watching a friend use `gh copilot suggest` to
generate a file processing command. The friend skips `gh copilot
explain`. The command exits 0. The friend moves on. Three days
later: the wrong files were processed. Not visibly broken. Silently
wrong. Seth already knows something is wrong before he has the
vocabulary to name it.

<!-- → [DIAGRAM: Seth's arc from observer to practitioner —
two-point timeline showing "watches friends" → "builds the
discipline". Minimal. Editorial style. No color.] -->

**Core content:**
- Seth's observation: the silent failure
- Why the terminal is different from the editor (no undo, silent
  errors, scope surprises)
- What this book is and is not
- The three tools: `gh copilot suggest`, `gh copilot explain`,
  `gh copilot ask`
- How to read this book

**Assessable exercises:**
1. (Remember) Before reading Chapter 1: write down three terminal
   commands you have run in the last month. For each: could you
   explain exactly what it did to someone who had never seen it?
2. (Understand) What is the difference between a command that exits
   0 and a command that did what you meant? Write one paragraph.
3. (Understand) Name one terminal moment where you ran something
   without fully understanding it. What happened?

**Wayback Machine:** 🕰️ **Norbert Wiener** (1894–1964) — founder
of cybernetics. His question — what does the machine do to the
human who uses it? — is the question this chapter asks about
the terminal.

**Bridge:** The feeling Seth has is real. Chapter 1 gives it a
mechanism, a number, and a specific terminal failure mode.

---

### Chapter 1 — The Silent Failure: What's Actually Happening

**One-line:** The most dangerous terminal failure is the one
that doesn't look like a failure at all.

**Learning outcomes:**
1. (Understand) Explain why exit 0 does not guarantee correct
   behavior in a shell command.
2. (Analyze) Distinguish between a command that failed visibly
   and one that failed silently.
3. (Evaluate) Assess their own recent terminal AI use against
   this distinction.

**Opening:** The Bastani finding stated for terminal work: the
student who delegates command generation without understanding
what was generated scores dramatically lower when asked to
operate the terminal unassisted. Not slightly worse. Demonstrably
worse. And the terminal version of the homework/quiz gap has
a cost the editor version doesn't — a wrong command can delete
files.

<!-- → [TABLE: Silent failure taxonomy — four rows. Row 1:
visible failure (non-zero exit, error message). Row 2: silent
wrong scope (processes more than intended). Row 3: silent wrong
target (right operation, wrong files). Row 4: silent wrong timing
(runs at wrong moment in pipeline). Each row: what it looks like,
what it costs, how to catch it.] -->

**Core content:**
- What exit 0 means and what it doesn't
- The four categories of terminal silent failure
- Why the Kosmyna EEG result applies to terminal work:
  the student who delegates without understanding atrophies
  the mental model of what the terminal is doing
- The three low-scoring interaction patterns from the Anthropic
  RCT applied to terminal AI: AI Delegation (run without explain),
  Progressive AI Reliance (never build the mental model),
  Iterative AI Debugging (fix errors without understanding why)
- Why `gh copilot explain` is not optional: it is the point
  where the student builds or fails to build the mental model

<!-- → [DIAGRAM: The silent failure — two-path diagram. Path A:
suggest → explain → verify → understand → build capability.
Path B: suggest → run → success (apparent) → no understanding
→ atrophy. Editorial style. No color.] -->

**Worked example:** Two students, same task: "archive log files
older than 7 days." One uses `gh copilot explain` before running.
One doesn't. The command runs for both. Six weeks later: one
can write and audit shell automation. One cannot.

**Assessable exercises:**
1. (Apply) Run `gh copilot suggest` on a task you know well.
   Before running the output, explain what each part of the
   generated command does. Were you right?
2. (Analyze) Given two command transcripts (provided), identify
   which student is building understanding and which is borrowing.
3. (Evaluate) Design a personal rule for your `gh copilot suggest`
   use that would prevent silent failure.

**Wayback Machine:** 🕰️ **William James** (1842–1910) — first
described habit formation as the nervous system's mechanism for
consolidating repeated struggle into durable capability. His
account is the mechanism the silent failure breaks.

**Bridge:** The reader knows the risk. They don't yet know which
cognitive capacities are at stake.

---

### Chapter 2 — What You're Actually Good At (And What `gh copilot suggest` Is Better At)

**One-line:** Pattern completion is the CLI's domain. Scope
judgment is yours. Knowing which is which is the whole game.

**Learning outcomes:**
1. (Understand) Distinguish pattern completion (where the CLI
   is superhuman) from scope judgment (where the CLI is
   structurally blind).
2. (Apply) Classify a set of terminal tasks as CLI work or
   human work.
3. (Analyze) Identify the specific supervisory capacity being
   exercised at a given step in a shell build.

**Opening:** Seth asks `gh copilot suggest` to write a command
that finds large files. The CLI produces a correct `find` command
— correct syntax, correct flags, correct output format. It has
no idea what "large" means in Seth's project context, what
directories should be excluded, or what he intends to do with
the results. That gap is the chapter.

<!-- → [TABLE: Division of labor — two columns: CLI does /
Human does. CLI: pattern completion, syntax generation, flag
lookup, command structure. Human: scope definition, target
specification, exclusion decisions, intent verification,
consequence assessment.] -->

**Core content:**
- What pattern completion means and why `gh copilot suggest`
  is superhuman at it
- What scope judgment means: the five capacities in plain
  terminal language
- Why the CLI is structurally blind to your filesystem, your
  Git history, and your intent
- The dangerous middle: tasks that look like pattern work but
  require scope judgment
- The asymmetry: `gh copilot suggest` generates faster than you
  can verify; that gap won't close; verification is your job

**Worked example:** The same file archiving task analyzed twice
— once with the CLI running unguarded, once with the student
exercising each supervisory capacity explicitly. Same CLI output.
Different results.

**Assessable exercises:**
1. (Apply) Given a list of 10 terminal tasks, classify each as
   CLI work, human work, or "dangerous middle."
2. (Analyze) Read a provided CLI transcript. Identify every
   moment where scope judgment should have been exercised
   but wasn't.
3. (Create) Write your own labor separation rule for a shell
   project you are working on.

**Wayback Machine:** 🕰️ **Frederick Winslow Taylor** (1856–1915)
— first systematic analyst of the division of labor between
human judgment and mechanical execution. His question — which
cognitive work belongs to the engineer and which to the machine?
— is this chapter's question for the terminal.

**Bridge:** The reader can name the capacities. Chapter 3
explains why school isn't teaching terminal discipline at all.

---

### Chapter 3 — The Terminal Gap: Why You're On Your Own

**One-line:** Your teachers are teaching you to code. Nobody
is teaching you to conduct the terminal. That gap is exactly
where AI is most dangerous.

**Learning outcomes:**
1. (Understand) Explain the terminal gap and why it produces
   a specific kind of risk for technically fluent students.
2. (Analyze) Distinguish terminal fluency from terminal
   judgment.
3. (Evaluate) Assess their own terminal judgment in a domain
   they use AI assistance for regularly.

**Opening:** The frustration named directly. Your CS class
teaches syntax. Your teachers may not use the terminal the
way you use it. The curriculum doesn't address `gh copilot
suggest` at all. You need a discipline that works without
institutional scaffolding.

**Core content:**
- Why terminal fluency without judgment is the specific
  danger zone
- The hallucination problem in the terminal: `gh copilot suggest`
  produces confident commands for environments it has never seen
- Why the answer is not to use `gh copilot suggest` less but
  to build the explain → verify discipline
- What "terminal judgment" means in practice and how you
  build it

**Wayback Machine:** 🕰️ **Ivan Illich** (1926–2002) — argued
that tools become counterproductive when they outpace the human
capacity to use them wisely. His concept of "counter-productivity"
is the terminal gap stated as philosophy.

**Bridge:** Chapter 4 introduces conducting.

---

## ACT TWO — THE DISCIPLINE
*Chapters 4–10: From awareness to operational framework*

---

### Chapter 4 — Conducting, Not Running: The Core Idea

**One-line:** Using `gh copilot suggest` as a conductor. The CLI
generates the command. You decide whether it runs.

**Learning outcomes:**
1. (Understand) Explain the difference between running a `gh
   copilot suggest` output and conducting a terminal build.
2. (Apply) Use the suggest → explain → verify gate on a real
   terminal task.
3. (Understand) Explain what a handoff condition is in the
   terminal context and why it matters.

**Opening:** The orchestra metaphor in Seth's voice. The CLI
is the orchestra. They are excellent. They will play exactly
what they understood you to mean. The gap between what you
meant and what they understood is where files get deleted.

<!-- → [DIAGRAM: The suggest → explain → verify gate. Human:
formulate task. gh copilot suggest: generate command.
gh copilot explain: explain it. Human: evaluate explanation
against intent. Verify in sandbox. Run. No command runs
without explanation reviewed. Editorial style.] -->

**Core content:**
- `gh copilot suggest`: what it does, what it cannot see
- `gh copilot explain`: not optional — this is where you
  build or fail to build the mental model
- `gh copilot ask`: for interrogating the problem space
  before suggesting anything
- The gate: no command runs until you have read and
  understood the `gh copilot explain` output
- What a handoff condition is: not "it ran without errors"
  but a specific, testable condition that must be true
- The dry-run principle: when possible, verify in sandbox
  before production

**Worked example:** One task done twice. First: `gh copilot suggest`
output run directly. Second: suggest → explain → verify gate
applied. Same CLI. Completely different outcomes.

**Assessable exercises:**
1. (Apply) Take a task you've used `gh copilot suggest` for in
   the past week. Apply the full gate retroactively: explain
   the command it generated. Would you have caught anything?
2. (Apply) Write a handoff condition for a terminal task in a
   current project.
3. (Analyze) Given a provided CLI transcript, identify where
   the gate was skipped and what broke.

**Wayback Machine:** 🕰️ **Herbert Simon** (1916–2001) — formalized
bounded rationality: working within real cognitive limits by
designing systems that extend those limits. The suggest →
explain → verify gate is bounded rationality applied to
terminal AI.

**Bridge:** The reader has the metaphor and the basic mechanics.
Chapter 5 names the five things the human must never delegate.

---

### Chapter 5 — The Five Supervisory Capacities

**One-line:** These are the five things you do that `gh copilot
suggest` cannot. Name them. Practice them. Never delegate them.

**Learning outcomes:**
1. (Remember) Name and define the five supervisory capacities.
2. (Apply) Identify which supervisory capacity is being
   exercised at each step of a provided terminal build sequence.
3. (Analyze) Diagnose a build that went wrong by identifying
   which supervisory capacity was absent.

**Opening:** Seth mid-build. Something is wrong with the
`gh copilot suggest` output. It explains correctly. It runs
without error. He almost deploys it. What capacity would have
caught this? Plausibility auditing — the feeling that the
scope is wider than he intended.

<!-- → [DIAGRAM: Five supervisory capacities as a five-column
layout. Each: abbreviation, plain name, terminal-specific
one-sentence definition.] -->

**Core content:**

**[PA] Plausibility Auditing:** Hearing the wrong note in a
terminal command before running it. "The explain output says
this finds files older than 7 days. But in my project, 7 days
includes files that are still active. Something feels wrong."

**[PF] Problem Formulation:** Deciding what the task IS before
`gh copilot suggest` sees it. "Am I archiving, deleting, or
moving? Those are different commands with different consequences."
`gh copilot ask` is the tool for this step.

**[TO] Tool Orchestration:** Choosing which of the three CLI
tools in what order, with what context pasted in from CLI.md.
Deciding when to use `gh copilot suggest` vs. write the command
manually. Deciding when a task is too high-stakes for AI
generation at all.

**[IJ] Interpretive Judgment:** Supplying meaning the CLI's
explanation cannot supply. "This command is technically correct
for the general case. In my specific repo structure, it will
miss the files in subdirectories."

**[EI] Executive Integration:** Holding the whole build toward
a single goal. "Three prompts ago we established this script
should never touch the main branch. This new suggestion is
operating on main. Stop."

**Wayback Machine:** 🕰️ **Douglas Engelbart** (1925–2013) —
argued that computers should augment human intellect, not replace
it. The five supervisory capacities are his augmentation
framework applied to the terminal.

**Bridge:** Chapter 6 introduces CLI.md — the file that makes
every subsequent terminal session smarter.

---

### Chapter 6 — CLI.md: Your Terminal Constitution

**One-line:** CLI.md is the file you maintain and paste from.
It is the difference between a `gh copilot suggest` session
that knows your project and one that guesses.

**Learning outcomes:**
1. (Understand) Explain what CLI.md is, what it contains,
   and why it has no automatic injection mechanism.
2. (Apply) Write a CLI.md for a student terminal project.
3. (Analyze) Distinguish CLI.md content (persistent project
   knowledge) from command-level context (pasted per invocation).

**Opening:** Seth starts his second `gh copilot suggest` session.
The CLI has no memory of the first one. He types the same
context he typed yesterday. This chapter ends that.

<!-- → [DIAGRAM: CLI.md in the workflow — created before build,
consulted before each gh copilot suggest invocation, updated
after each session. Contrast: without CLI.md (CLI guesses) vs.
with CLI.md context pasted (CLI knows the project).] -->

**Core content:**
- What CLI.md is: a markdown file the student maintains and
  pastes from — project conventions, environment quirks,
  "never do X" rules, lessons learned from failures
- Why there is no automatic injection: unlike CLAUDE.md or
  AGENTS.md, CLI.md requires active human decision about
  what context to provide per invocation. This is the feature,
  not the limitation — it forces supervisory capacity [TO]
  on every use.
- The four-section format:
  1. Project overview (one paragraph)
  2. Environment facts (OS, shell, directory structure,
     important paths)
  3. Command conventions and exclusions ("never touch X",
     "always use Y flag")
  4. Lessons learned (what went wrong, what the fix was)
- How to paste from CLI.md effectively: paste the relevant
  sections, not the whole file
- Updating CLI.md after every session: if `gh copilot suggest`
  made the same mistake twice, add the fix to CLI.md

<!-- → [TABLE: CLI.md include/exclude — two columns. Include:
environment facts, project-specific conventions, known dangerous
patterns, lessons from failures. Exclude: general shell knowledge
the CLI already has, constantly changing state, personal notes
unrelated to the project.] -->

**Worked example:** Seth writes his first CLI.md for a file
automation project. Four sections. In the next session, he
pastes the environment facts and conventions into his `gh copilot
suggest` prompt. The suggestions immediately match his project
structure.

**Assessable exercises:**
1. (Apply) Write a CLI.md for a current terminal project.
2. (Analyze) Run `gh copilot suggest` without CLI.md context,
   then with it, on the same task. Document three specific
   differences in the generated commands.
3. (Evaluate) After one week of use: what did you add to
   CLI.md that you didn't know to include on day one?

**Wayback Machine:** 🕰️ **Donald Knuth** (born 1938) — inventor
of literate programming: writing the explanation alongside the
code so both human and machine are served. CLI.md is literate
programming applied to AI-assisted terminal work.

**Bridge:** The reader has CLI.md. Chapter 7 teaches them to
formulate problems before `gh copilot suggest` sees them.

---

### Chapter 7 — Problem Formulation: The Mission Before the Command

**One-line:** The most expensive mistake in a terminal build
happens before the first `gh copilot suggest` invocation.
Formulate the problem first.

**Learning outcomes:**
1. (Understand) Explain why problem formulation is the most
   important step in a conducted terminal build.
2. (Apply) Use `gh copilot ask` to interrogate a problem
   before writing a single suggest prompt.
3. (Analyze) Identify the sections of a problem brief most
   likely to reveal a formulation gap.

**Opening:** Seth starts a build. Thirty minutes in, he
realizes he is automating the wrong thing. Not the wrong
command — the wrong task entirely. The decision he needed
to make was at the beginning, not the middle.

<!-- → [DIAGRAM: The problem formulation gate — one sentence
naming what the script does, what it touches, and what it
must never touch. Below the gate: suggest prompts. Above
the gate: nothing.] -->

**Core content:**
- The one-sentence formulation: what does this script do,
  what does it touch, what must it never touch?
- Using `gh copilot ask` for interrogation before suggestion:
  "What would I need to know about my directory structure
  before writing a file archiving script?" is a formulation
  question, not a suggest prompt
- The minimum viable brief: task statement, scope boundaries,
  exclusions, success definition
- The planning gate: nothing goes to `gh copilot suggest`
  until the formulation has passed the one-sentence test

**Wayback Machine:** 🕰️ **Frederick Brooks** (1931–2022) —
established that the most expensive bugs come from building
the wrong thing. His principle that design precedes
implementation is the problem formulation gate for the terminal.

**Bridge:** The reader has a problem formulation. Chapter 8
teaches them to write `gh copilot suggest` prompts that are
specifications, not requests.

---

### Chapter 8 — Writing `gh copilot suggest` Prompts That Are Specifications

**One-line:** "Archive log files" is not a prompt. A prompt
names the files, the destination, the exclusions, and what
must not be touched.

**Learning outcomes:**
1. (Understand) Distinguish a prompt (request) from a
   specification (complete task definition).
2. (Apply) Rewrite a weak `gh copilot suggest` prompt as
   a complete specification using the five-element format.
3. (Analyze) Identify what is missing from a set of provided
   prompts that would cause the CLI to produce incorrect output.

**Opening:** Side by side: the same task written as a prompt
and as a specification. The CLI's output for each. The
difference is not the CLI — it's the precision of the
instruction.

<!-- → [TABLE: Prompt vs. specification — two columns, five rows.
Each row: one element. Left: weak prompt version. Right:
specification version. Applied to terminal tasks.] -->

**Core content:**
- The five elements of a `gh copilot suggest` specification:
  1. The specific operation (not "handle files" but "move")
  2. The target scope (which files, which directories,
     what pattern)
  3. The exclusions (what must not be touched)
  4. The output format (what done looks like)
  5. The negative constraint (what the command must not do)
- Why the CLI has no memory between invocations: every
  specification must contain the constraints that govern it
- Give `gh copilot suggest` a way to generate a verifiable
  command: include `--dry-run` flags, `echo` prefixes, or
  `-print` flags in your specification so the output can
  be inspected before execution
- Pasting from CLI.md as part of the specification prompt

**Wayback Machine:** 🕰️ **Ada Lovelace** (1815–1852) — wrote
the first computer program: a precise specification of
operations in dependency order. Her Notes are the conceptual
ancestor of the five-element specification format.

**Bridge:** The reader can write specifications. Chapter 9
addresses what happens when the specification is right and
the command is still wrong.

---

### Chapter 9 — Handoff Conditions and the Dangerous Middle

**One-line:** Not "it ran without errors." A specific, testable
condition that must be true before the next step begins —
because the terminal's silent failure mode is the most
dangerous one.

**Learning outcomes:**
1. (Understand) Explain what a handoff condition is for a
   terminal build and why exit 0 fails as one.
2. (Apply) Write handoff conditions for a set of provided
   terminal tasks.
3. (Analyze) Identify the dangerous middle — where `gh copilot
   explain` describes accurately and exit 0 confirms success
   but the command did the wrong thing.

**Opening:** Seth approves a `gh copilot suggest` output. It
explains correctly. It exits 0. The handoff condition he
wrote was "runs without errors." Six days later: silent wrong
scope. The condition that wasn't met was the one he didn't
know to check. That's the dangerous middle.

<!-- → [DIAGRAM: The handoff condition as a gate between build
steps. Step N → [Handoff condition check: specific, testable,
not "exit 0"] → Step N+1. If check fails: revert and respecify.] -->

**Core content:**
- What a handoff condition is in the terminal: specific,
  testable, binary — and never "ran without errors"
- The dangerous middle for terminal work: commands that
  `gh copilot explain` describes accurately, that exit 0,
  that match the specification literally, but whose scope
  was wider than intended
- Common dangerous middle patterns:
  - `find` that matches more files than intended
  - `rm` with a glob that expands unexpectedly
  - `git` that operates on the wrong branch
  - `sed` or `awk` that modifies in-place without backup
- The dry-run verification: always run with `--dry-run`,
  `echo`, or `-print` first when available. When not
  available, test on a copy.
- When output fails the handoff condition: revert and
  respecify. Do not correct forward.
- After two failed corrections: restate the problem from
  scratch with a better specification.

<!-- → [TABLE: Strong vs. weak handoff conditions for terminal
tasks. Five examples. Left: weak (exit 0, no errors). Right:
strong (specific count, specific files, specific state).] -->

**Wayback Machine:** 🕰️ **Grace Hopper** (1906–1992) — insisted
on explicit correctness criteria in software. Her insistence
that "done" must be defined before it can be verified is the
handoff condition principle at the terminal.

**Bridge:** The reader has the full framework for terminal
builds. Chapter 10 applies the same discipline to creative
work.

---

### Chapter 10 — When the Build Is Creative: Scripts with Aesthetic Choices

**One-line:** The terminal is not just for automation. When
your script has aesthetic choices — output format, naming
conventions, interaction design — the creative judgment stays
yours.

**Learning outcomes:**
1. (Understand) Explain how the fluency trap manifests in
   creative terminal work.
2. (Apply) Apply the CLI.md creative section to a script
   that has output formatting choices.
3. (Analyze) Distinguish aesthetic judgment (irreducibly human)
   from mechanical execution (CLI's domain) in a provided
   terminal build.

**Opening:** Seth builds a script that generates a weekly
summary report. `gh copilot suggest` produces the output
formatting. It's clean. It's efficient. It has no voice,
no stance, no genuine human choice about what the report
should feel like. The output is technically correct and
completely generic.

**Core content:**
- Creative terminal work: scripts that produce human-facing
  output (reports, summaries, notifications)
- The fluency trap at the terminal: letting the CLI make
  aesthetic choices (output format, naming, structure) means
  the output belongs to the model, not to you
- Adding a creative section to CLI.md: what the output should
  look like, what conventions matter, what the voice is
- The labor separation: `gh copilot suggest` handles the
  mechanics; you keep the aesthetic choices

**Wayback Machine:** 🕰️ **Sol LeWitt** (1928–2007) — the person
who holds the intent and writes the instruction is the author,
regardless of who executes. The creative section of CLI.md is
this principle applied to the terminal.

**Bridge:** The reader has the full discipline. Chapter 11
is the planning phase of their first complete build.

---

## ACT THREE — THE BUILD
*Chapters 11–14: From framework to first fully conducted terminal build*

---

### Chapter 11 — Planning Your First Conducted Build

**One-line:** Before `gh copilot suggest` runs a single
command, you know exactly what you are building, why, and
which steps belong to you.

**Learning outcomes:**
1. (Apply) Complete a problem formulation and CLI.md for a
   student-scale shell project using `gh copilot ask`.
2. (Apply) Generate an execution plan using `gh copilot ask`
   and review it before any `gh copilot suggest` invocations.
3. (Analyze) Identify the three steps in the build most likely
   to hit the dangerous middle.

**Opening:** Seth planning his first fully conducted build.
The `gh copilot ask` interrogation. The CLI.md populated.
The execution plan reviewed. The moment he realizes: he is
not thinking about `gh copilot suggest` yet. He is thinking
about the problem. That is the discipline working.

<!-- → [DIAGRAM: The planning sequence — gh copilot ask
interrogation → problem formulation → CLI.md populated →
execution plan → review and approve → gh copilot suggest
invocations. Phase gates labeled.] -->

**Core content:**
- The planning sequence: `gh copilot ask` interrogation →
  one-sentence formulation → CLI.md populated → execution
  plan → review → `gh copilot suggest` invocations
- How to scope a student shell project for a first conducted
  build
- Reading a `gh copilot ask` plan: what to check, what
  to correct, what constitutes approval
- The planning gate: what must be true before the first
  `gh copilot suggest` invocation

**Wayback Machine:** 🕰️ **Christopher Alexander** (1936–2022) —
argued that good design begins with a clear statement of the
problem. His principle is the problem formulation gate for
terminal builds.

**Bridge:** The plan is complete and approved. Chapter 12
executes it.

---

### Chapter 12 — Running the Build: CLI Tasks and Human Tasks

**One-line:** The plan is approved. Now you execute it —
one command at a time, with the suggest → explain → verify
gate applied at every step.

**Learning outcomes:**
1. (Apply) Execute a conducted terminal build sequence,
   completing CLI tasks and human tasks in dependency order.
2. (Apply) Apply each of the five supervisory capacities
   at the steps requiring them.
3. (Analyze) Identify when a build is going off-script
   and stop before it breaks.

**Opening:** Seth mid-build. The plan is approved. He is
doing his job — not running output, but evaluating it
against the handoff condition. He rejects one command.
Respecifies. The revision passes. The build continues.
This is what conducting feels like.

**Core content:**
- Running `gh copilot suggest` against the approved plan,
  one step at a time
- Always paste relevant CLI.md sections as context
- Always run `gh copilot explain` before executing
- Human tasks labeled by supervisory capacity at each step
- When a command fails the handoff condition: revert and
  respecify. Do not correct forward.
- When output explains correctly and still feels wrong:
  plausibility auditing — trust the feeling, investigate it
- The scope creep moment: `gh copilot suggest` proposes
  an "while we're here" improvement. Add to CLI.md for
  later. Do not execute now.
- After two failed corrections: restate the problem from
  scratch.

**Wayback Machine:** 🕰️ **W. Edwards Deming** (1900–1993) —
Plan-Do-Check-Act: quality built into a process through
verification at every step. The suggest → explain → verify
gate is Deming's cycle applied to the terminal.

**Bridge:** The build is done when it passes the handoff
conditions. Chapter 13 defines what "done" actually means.

---

### Chapter 13 — Verification: How You Know It Works

**One-line:** The build is done when it passes the handoff
conditions — not when `gh copilot suggest` says it's done,
not when it exits 0.

**Learning outcomes:**
1. (Apply) Run a structured verification pass on a completed
   terminal build using explicit criteria from the
   problem formulation.
2. (Analyze) Distinguish build failures from silent failures
   that look like successes.
3. (Evaluate) Produce a post-build assessment.

**Opening:** Seth's build exits 0 across every step. He is
about to declare it done. He runs the verification pass —
the one he almost skipped. He finds a silent scope error
that exit 0 never caught. The error is not in the commands.
It's in the original formulation.

<!-- → [DIAGRAM: The verification sequence — three passes.
Pass 1: mechanical verification (exit 0, expected output).
Pass 2: scope verification (right files, right directories,
right count). Pass 3: intent verification (does the result
match what I said I needed). Binary result at each.] -->

**Core content:**
- Verification against intent, not just exit status
- The three-pass verification sequence: mechanical,
  scope, intent
- The dry-run as a verification tool: run with `--dry-run`
  or `echo` prefix on the full dataset before production
- The post-build learning document

**Wayback Machine:** 🕰️ **Barbara Liskov** (born 1939) —
formalized the behavioral contract: "correct" must be
defined before it can be verified. Her contribution is
the intent verification pass stated as a principle.

**Bridge:** The reader has the discipline. Chapter 14
hands them the build.

---

### Chapter 14 — Your First Full Build: From Problem to Verified Output

**One-line:** You have the discipline. Here is the project.
Conduct it.

**Learning outcomes:**
1. (Create) Plan, execute, and verify a student-scale shell
   project using the complete conducting framework.
2. (Evaluate) Assess their own build against the five
   supervisory capacities.
3. (Create) Produce a post-build document.

**Opening:** Not Seth's build. Yours. The chapter gives the
student the project brief, the tools, and the sequence.
Everything else is their decision.

**Core content:**
- The project brief: a student-scale shell automation
  project requiring all five supervisory capacities
- The complete sequence: `gh copilot ask` interrogation →
  problem formulation → CLI.md populated → execution plan
  → review → `gh copilot suggest` + explain + verify at
  every step → three-pass verification → post-build document
- What success looks like: a build where you can explain
  every command that ran

**Terminal deliverable:** The reader's first fully conducted
terminal build — planned with a CLI.md and formulation,
executed with the suggest → explain → verify gate, verified
against intent, documented in a post-build learning record.

<!-- → [DIAGRAM: The complete arc — four milestones:
Problem named / Discipline learned / First build planned /
First build verified. Editorial style.] -->

**Closing:**
"You built it. You can explain every command that ran.
You know what every flag does. You know why every scope
decision was made. You know what you would do differently.
That is what the terminal feels like when you are the
conductor. That is what `gh copilot suggest` is for."

**Wayback Machine:** 🕰️ **John Dewey** (1859–1952) —
learning is not the transmission of information but the
transformation of the learner through purposeful experience.
The post-build learning document is the record that the
experience changed the person, not just the repository.

---

# PART 9 — CHAPTER ANATOMY TEMPLATE

All 14 chapters follow this structure:

1. **One-line descriptor** (capability build, not topic)
2. **Learning outcomes** (3–5, Bloom's level labeled)
3. **Chapter opening** (failure case or Seth narrative)
4. **Figure comments** — `<!-- → [DIAGRAM: ...] -->` or
   `<!-- → [TABLE: ...] -->` embedded at point of use
5. **Core content sections** (4–6)
6. **Worked example** (Seth's story or provided transcript)
7. **Assessable exercises** (minimum 3; at least one at
   Apply or above; at least one requiring production)
8. **Links** (where applicable: cli.github.com, docs.github.com)
9. **AI Wayback Machine** — one pre-2000 figure per chapter
10. **Bridge** (one sentence)

**Seth's voice rule:** Chapters 0–3 and 11–12 are primarily
Seth's narrative voice. Chapters 4–10 are framework-forward
with Seth as illustration. Chapters 13–14 transition to the
reader as primary actor.

---

# PART 10 — CASE STUDY STRATEGY

## Domain coverage map

| Domain | Chapters | Primary use |
|---|---|---|
| File archiving / log management | 0, 1, 9 | Seth's observation; dangerous middle |
| Git automation | 2, 5, 8 | Scope judgment; supervisory capacities |
| Shell script automation | 7, 11 | Problem formulation; planning |
| Report generation | 10 | Creative terminal build |
| Student shell project | 12, 13, 14 | Full build arc |

## Case escalation

Act One: observation only — Seth watching, no build.
Act Two: single-concept worked examples.
Act Three: Seth's real build session — real prompts, real
`gh copilot suggest` and `gh copilot explain` outputs, one
documented failure and revision.

---

# PART 11 — HARD TOPICS, CONTESTED CLAIMS, AGING RISK

## Contested claims

| Claim | Status | Book's position |
|---|---|---|
| CLI.md has no automatic injection | Accurate as of writing | "Currently" qualifier; noted in Ch 6 |
| gh copilot suggest is limited to terminal tasks | Accurate | Scope stated clearly in Ch 1 |
| Five supervisory capacities are permanent | Emerging | "Currently requires human judgment" |
| Exit 0 guarantees correctness | False — this is the book's thesis | Stated explicitly in Ch 1 |

## Hard chapters

**Chapter 9 (Dangerous Middle):** Must produce genuine
discomfort. The worked example where exit 0 hides a scope
error requires a real `gh copilot suggest` session, not
a hypothetical.

**Chapter 1 (Silent Failure):** The empirical foundation.
The Bastani finding and Kosmyna EEG must be stated precisely.

## Aging risk

| Content type | Risk | Review cadence |
|---|---|---|
| gh copilot suggest / explain / ask syntax | Medium | Before each edition |
| CLI.md concept (manual paste approach) | Low — tool-agnostic by design | On major tool change |
| Five supervisory capacities | Low | On major framework revision |
| Bastani / Kosmyna / Anthropic RCT numbers | Low-Medium | On new major studies |
| Seth's narrative | None | N/A |

**Aging mitigation:** The conducting discipline is
tool-agnostic. Even if `gh copilot suggest` changes its
interface, the suggest → explain → verify gate applies
to any AI-assisted terminal tool. CLI.md is a practice,
not a file format.

---

# PART 12 — MARKET POSITIONING SUMMARY

## The gap this book fills

No book teaches the conducting discipline for AI-assisted
terminal work. Editor-based AI books (Claude Code, Codex)
never reach the terminal. Generic shell scripting books
predate AI assistance entirely. This book is the only
practitioner handbook for the student who uses `gh copilot
suggest` and needs to know that exit 0 is not enough.

## What makes this book distinctive in the series

- The dangerous middle is different: silent scope error vs.
  semantic code error vs. pedagogically wrong feedback
- CLI.md is manually maintained, which makes the supervisory
  discipline more visible than in the other books
- The stakes are higher: terminal errors can be irreversible

---

# PART 13 — FEATURE LIST

| Feature | Priority | Production effort |
|---|---|---|
| 14-chapter architecture | ESSENTIAL | Low |
| Three-act learning arc | ESSENTIAL | Low |
| Seth's co-authored narrative | ESSENTIAL | Medium |
| Five supervisory capacities framework | ESSENTIAL | Low |
| suggest → explain → verify gate | ESSENTIAL | Low |
| CLI.md full chapter treatment | ESSENTIAL | Medium |
| Assessable exercises (3+ per chapter) | ESSENTIAL | Medium |
| Terminal deliverable (post-build document) | ESSENTIAL | Low |
| AI Wayback Machine (14 figures) | IMPORTANT | Low |
| Worked examples (Seth's real builds) | IMPORTANT | High |
| Figure comments for Cowork enrichment | IMPORTANT | Medium |
| Appendix A (gh copilot CLI quick reference) | IMPORTANT | Low |
| Appendix B (CLI.md template) | IMPORTANT | Low |
| Appendix C (post-build document template) | IMPORTANT | Low |

---

# PART 14 — OUT OF SCOPE

| Topic | Reason | Covered better in |
|---|---|---|
| GitHub Copilot editor features | Editor scope | Claude Code / Codex students books |
| Full shell scripting tutorial | Prerequisite scope | Any shell scripting book |
| GitHub CLI features beyond copilot | Outside conducting discipline | docs.github.com |
| Advanced Git internals | Too complex for first build | Dedicated Git books |
| CI/CD pipeline automation | Second edition scope | Future edition |

---

# PART 15 — ADOPTION RISK REGISTER

| # | Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| 1 | "Just give me the commands" reader | High | Medium | Seth's Ch 0 narrative must land; the silent failure story is the hook |
| 2 | Aging risk: gh copilot syntax changes | Medium | Medium | Conducting discipline in main text; syntax in appendix |
| 3 | CLI.md concept seen as extra work | Medium | Medium | Ch 6 shows the payoff immediately; second session is demonstrably better |
| 4 | Seth's co-authorship scope | Medium | Medium | Seth drafts Ch 0, narrative sections, real build logs |
| 5 | Book seen as too short vs. series | Low | Low | Shorter because the tool is sharper; stated explicitly in preface |

---

# PART 16 — OPEN QUESTIONS

| # | Question | Stakes | Decision deadline | Owner |
|---|---|---|---|---|
| 1 | What is Seth's specific terminal build project for Chapters 11–13? Real, reproducible. | Worked example quality | Before chapter drafting | Seth + Author |
| 2 | Should CLI.md have a recommended file extension or live in a specific directory? | Consistency across books | Before Ch 6 drafting | Author |
| 3 | Seth's co-authorship credit: cover attribution and royalty structure? | Legal; contract | Before contract | Author + Publisher |
| 4 | Should the book include a Windows PowerShell track alongside bash? | Accessibility | Before production | Author |
| 5 | gh copilot suggest / explain / ask — confirm current syntax and flags before Chapter 4 drafting | Accuracy | Before Act Two drafting | Author |

---

## Appendix A — GitHub Copilot CLI Quick Reference

| Command | What it does | When to use it |
|---|---|---|
| `gh copilot suggest` | Generate a shell command from natural language | After problem is formulated and CLI.md context is ready to paste |
| `gh copilot explain` | Explain a shell command in plain language | Always, before running any suggest output |
| `gh copilot ask` | Ask a general coding or terminal question | Problem formulation; interrogating the task before suggesting |
| `gh copilot config` | Configure CLI settings | Initial setup |

Full documentation: [cli.github.com/manual/gh_copilot](https://cli.github.com/manual/gh_copilot)

---

## Appendix B — CLI.md Template

```markdown
# CLI.md — [Project Name]

## Project overview
[One paragraph: what this project does, what it automates,
what the output is]

## Environment
- OS: [e.g., macOS 14, Ubuntu 22.04, Windows 11 + WSH]
- Shell: [bash / zsh / PowerShell]
- Key directories: [list important paths]
- Important tools installed: [list relevant tools]

## Command conventions
- Always: [e.g., use --dry-run before production runs]
- Never: [e.g., never touch /archive/locked/]
- Prefer: [e.g., use find over ls for file operations]

## Known dangerous patterns
[List commands or patterns that caused problems, with context]

## Lessons learned
[Date + what went wrong + what the fix was]
```

---

## Appendix C — Post-Build Learning Document Template

Five sections. One document. Honest.

1. What I built (one paragraph, plain language)
2. What I delegated to `gh copilot suggest` and why
3. What I kept for myself and why
4. What I learned that I didn't know before
5. What I would do differently

---

## Series Links

[cli.github.com](https://cli.github.com) ·
[docs.github.com/copilot](https://docs.github.com/en/copilot) ·
[brutalist.art](https://brutalist.art) ·
[irreducibly.xyz](https://irreducibly.xyz) ·
[nikbearbrown.com](https://nikbearbrown.com)

---

*Full TOC Draft v1.0 — compiled from all phase outputs*
*All phases complete: Vision (i1–i4), Learning Architecture
(l1–l4), Chapter Architecture (c1–c4), Scope & Market (m1–m4)*
*One blocker before production: OQ-1 (Seth's terminal build
project for Chapters 11–13)*
