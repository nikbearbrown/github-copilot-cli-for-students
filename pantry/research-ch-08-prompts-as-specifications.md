# Research: Chapter 8 — Writing `gh copilot suggest` Prompts That Are Specifications
## GitHub Copilot CLI for Students: A Practitioner's Guide

**Chapter one-line:** "Archive log files" is not a prompt. A prompt names the files, the destination, the exclusions, and what must not be touched.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Lovelace, Ada. *Notes on the Analytical Engine* (1843), in particular Note G.** Ada's notes contain what is widely recognized as the first computer program: an algorithm for computing Bernoulli numbers, specified as an explicit ordered sequence of operations with dependencies. The Notes are the conceptual ancestor of "specifications, not requests." Open access via the Babbage Engine project and various scholarly archives.
- **Liskov, Barbara H. and Jeannette M. Wing. "A Behavioral Notion of Subtyping." *ACM TOPLAS* 16, no. 6 (1994): 1811–1841.** The Liskov Substitution Principle articulated formally. The chapter borrows the underlying philosophy: a contract must be specified precisely enough that any conforming implementation produces equivalent behavior. A specification prompt is the contract; the generated command is the implementation.
- **Anthropic. "Prompt engineering" documentation (claude.com/docs/prompting).** The canonical reference for specification-shaped prompts in the LLM era. Useful for the technical infrastructure under the chapter without citing it as authority.
- **OpenAI. "GPT best practices" guide (platform.openai.com/docs).** Similar role to the Anthropic doc. The chapter doesn't need to cite both; one will do.
- **Wirth, Niklaus. "Program Development by Stepwise Refinement." *CACM* 14, no. 4 (1971): 221–227.** Wirth's argument that programs are developed through successive refinement of specifications. The five-element specification format is stepwise refinement done in one pass for a one-line command.

### Key empirical cases

- **The `rm -rf /` Steam Linux bug (Ch 2).** Specification failure: the prompt that generated the script did not name "what must not happen if `$STEAMROOT` is unset." Strong case for negative constraint in specification.
- **The AWS S3 outage of Feb 28, 2017 (Ch 1).** Specification failure: the engineer's intent ("take a few servers offline") was not specified precisely enough to prevent a wider scope. The killer detail: the command's *syntax* was valid for both the intended scope and the catastrophic scope.
- **A worked side-by-side for the chapter (Seth's domain).** Same task — "archive last semester's homework" — specified five ways:
  1. "Archive last semester's homework" (request).
  2. "Move my homework from last semester to an archive folder" (slightly better).
  3. "Move all directories under `~/homework/` whose modification date is before 2026-01-15 to `~/archive/spring-2025/`" (specific operation + scope).
  4. The above + "Do not touch directories named `.git/` or `node_modules/`" (with exclusions).
  5. The above + "Output: a list of moved directories. Do not run if any conflict in destination names." (with output format + negative constraint).
  
  The progression *is* the chapter's central worked example. Author can use as-is.

---

## 2. The Core Concept — State of the Field

### What is settled

- **Specification clarity correlates with output quality.** Established broadly in the prompt-engineering literature. The mechanism: LLMs generate plausible continuations of the prompt; a precise prompt narrows the plausibility space.
- **The model has no memory between invocations.** Without explicit context, every prompt starts fresh. The book's claim that the specification must contain its constraints is structural, not opinion.
- **Negative constraints matter.** "Do not touch X" is not implied by "operate on Y." The model can produce commands that do both unless told otherwise. The five-element format reflects this.

### What is disputed

- **The right level of formality.** Some practitioners advocate XML-tagged structured prompts; some advocate natural-language paragraphs; some advocate hybrid. The book recommends a simple five-element format because students must remember it, not because it is the only formalism that works.
- **Whether the model should be asked to "think step by step."** Mixed evidence. The book sidesteps this debate by focusing on the specification structure rather than reasoning elicitation.
- **Whether examples in the prompt help or hurt.** Useful for complex tasks; overhead for simple ones. The book recommends examples for commands with non-obvious flags, not for routine operations.

### What has changed recently (last 5 years)

- The 2024–2026 generation of CLIs treat structured prompts more reliably than the 2023 generation. The five-element format works across model generations; the book's specification advice is not vendor-specific.
- The post-January-2026 `copilot` CLI in plan mode generates a written plan before any command. The plan is a structured restatement of the prompt. A weak prompt produces a vague plan; a specification-shaped prompt produces a precise plan. This is implicit support for the chapter's argument.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 8 sits in Git automation.)

For a Git task, the five-element specification:

1. **Operation:** Squash (not "clean up", "tidy", or "consolidate").
2. **Scope:** The last 5 commits on the current branch.
3. **Exclusions:** Do not touch commits that are merge commits; do not touch commits older than the most recent main branch divergence point.
4. **Output format:** A single commit with a combined message containing all five original messages.
5. **Negative constraint:** Do not force-push; do not modify any branch other than the current one.

The same task as a one-line prompt ("squash my last few commits") would produce a syntactically valid command that might rewrite shared history, drop merge commits incorrectly, or modify the wrong branch.

Second example, for file operations:

1. **Operation:** Move.
2. **Scope:** All `.log` files modified more than 7 days ago, under `~/projects/`.
3. **Exclusions:** Skip files under any directory named `node_modules/` or `.git/`.
4. **Output format:** A count of moved files printed before exit; no files written outside `~/logs-archive/`.
5. **Negative constraint:** Do not delete; do not modify the source files in place; do not run if the destination directory is missing.

---

## 4. The Book's Thesis Connection

Ch 8 is the chapter that closes the loop between formulation (Ch 7) and execution (Ch 4's gate). The formulation produces *what the student wants*. The specification produces *what the prompt must contain to elicit a command that does what the student wants*. The gate then verifies that the generated command in fact matches.

The chapter's contribution:

1. **Specification is the bridge between intent and code.** A weak prompt cannot be saved by a strong explain step. The dangerous middle (Ch 9) lives in the gap between under-specified prompts and exit-0 outputs.
2. **The five elements are the operationalization of the supervisory capacities.** Operation = PF. Scope = IJ. Exclusions = PA. Output format = IJ + EI. Negative constraint = PA + EI. The chapter does not need to make this map explicit, but the author should know it's there.
3. **Specification is repeatable.** Once the student learns the five-element format, they apply it to every suggest. The discipline becomes the prompt structure.

Student-supplied capacity: every element of the specification is information the model cannot supply. Specification is the act of giving the model the context the model lacks.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Ada Lovelace (1815–1852).** Excellent fit. Keep as primary.

Candidates:

- **Ada Lovelace** (1815–1852, UK, mathematician). The named figure. *Notes on the Analytical Engine* (1843), Note G. The first program ever written was a specification in five-element form *avant la lettre*. Diversity: woman, British, 19th-century — strong diversity contribution. Famous to most students at least by name; less famous in the specific frame of "pioneer of specification." Wikipedia-accessible. **Substantive and diversity fit are both strong.**
- **Grace Hopper** (1906–1992, USA, computer scientist). COBOL, the A-0 compiler, "machine-independent programming languages." Hopper's career was about making specifications portable across machines. Better fit in Ch 9 (TIKTOC has her there — handoff conditions). Avoid double-booking.
- **Frances Allen** (1932–2020, USA, computer scientist). First woman Turing Award recipient (2006). Compiler optimization. Her work was about translating high-level specifications into efficient low-level code. Diversity: woman, American, mid-20th to 21st-century. Strong alternate; less name-recognition than Lovelace.

Recommendation: **keep Lovelace.** Note G *is* a specification. The historical precedent is too clean to swap. Allen would be a fine alternate if rebalancing demands.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
Ch 7's formulation. The reader has decided what they want. Ch 8 teaches how to convert that decision into a prompt the model can act on.

### Common misconceptions to disarm
- **"Specifications are for big projects."** No. Specifications are for any operation that can fail silently. A one-line `find` deserves the five-element treatment if it has the power to delete files.
- **"More words = better specification."** No. The five-element format is the *minimum*; quality is in the precision of each element, not the verbosity.
- **"I can just edit the generated command if it's wrong."** Sometimes. The dangerous middle is when the generated command is *not visibly wrong* and runs cleanly. Specification quality prevents the dangerous middle; correction-after-the-fact does not.

### Effective instructional sequences
- **Before/after pairs.** TIKTOC's central pattern. The chapter should contain five to seven before/after pairs, each annotated with which element of the specification was missing in the "before."
- **Reader rewrites their own prompt.** The Apply-level exercise. The reader brings a real prompt they've recently used; rewrites it as a five-element specification; runs both; compares outputs.
- **Negative-constraint emphasis.** Negative constraints are the element most readers underuse. Devote disproportionate attention to them in the chapter — "do not" is the most under-specified word in the chapter's domain.

### Known failure modes
- **Specification as boilerplate.** Readers will template the five elements and produce structurally complete but semantically empty prompts. The chapter must emphasize that each element should be *substantively decided*, not stylistically present.
- **Over-specification of trivial commands.** A reader who writes a five-element specification for `ls` has lost the plot. The chapter should give a heuristic: specify in proportion to consequence.
- **Confusion between specification and formulation.** Formulation (Ch 7) is the upstream work of deciding what to do. Specification (Ch 8) is the downstream work of converting that decision into a prompt. The chapter must not conflate them.

### What separates understanding from memorization
A reader who *understands* Ch 8 can take a prompt they've recently used and identify which of the five elements was implicit, explicit, or absent. A reader who memorized Ch 8 can recite the five elements without being able to spot which ones their actual prompts miss.

---

## 7. Representation and Display Research

TIKTOC specifies one figure:

- **`<!-- → [TABLE: Prompt vs. specification — two columns, five rows.] -->`** Worked content:

  | Element | Weak prompt | Specification |
  |---|---|---|
  | Operation | "Handle the log files" | "Move (not copy, not delete)" |
  | Scope | "Old log files" | "Files matching `*.log` modified ≥ 7 days ago, under `~/projects/`" |
  | Exclusions | (implicit) | "Skip any directory named `node_modules/` or `.git/`" |
  | Output format | (implicit) | "Print count of moved files; no files written outside `~/logs-archive/`" |
  | Negative constraint | (implicit) | "Do not delete; do not run if destination is missing" |

Editorial style. Two columns, five rows, with the "Element" row as a left-side label column.

No additional displays required.

---

## 8. Open Questions and Research Gaps

- **`gh copilot suggest` syntax post-January-2026.** Same as prior chapters. The chapter teaches a *format* — the format survives any tool. Syntax examples need a one-paragraph version note.
- **Specification templates as a reference card.** Author may want to produce a printable card with the five elements and one example. Strong reader-aid for chapter Apply.
- **Empirical specification-quality studies.** No published study cleanly measures specification format against output quality for terminal AI. The chapter's claim is consistent with prompt-engineering general findings. Acknowledge.

---

## 9. Sourcing Notes

- **Lovelace 1843**: scholarly editions available; the Anthropic Babbage Engine project hosts a clean version. The Notes are paginated; cite Note G specifically.
- **Liskov & Wing 1994**: ACM Digital Library; some open preprints exist.
- **Wirth 1971**: open access via *CACM* archive; ACM membership not required.
- **Anthropic and OpenAI prompt-engineering docs**: cite versions current at press.
