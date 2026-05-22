# Research: Chapter 7 — Problem Formulation: The Mission Before the Command
## GitHub Copilot CLI for Students: A Practitioner's Guide

**Chapter one-line:** The most expensive mistake in a terminal build happens before the first `gh copilot suggest` invocation. Formulate the problem first.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Brooks, Frederick P. "No Silver Bullet: Essence and Accidents of Software Engineering." *IEEE Computer* 20, no. 4 (1987): 10–19.** Brooks's central distinction — essential difficulty (specifying *what* should be built) vs. accidental difficulty (the mechanics of building it) — is the chapter's spine. LLMs reduce accidental difficulty. They do not reduce essential difficulty. Problem formulation *is* the essential work.
- **Brooks, Frederick P. *The Design of Design: Essays from a Computer Scientist*. Addison-Wesley, 2010.** Brooks's later work specifically on design as an act of formulation. The chapter borrows the framework directly.
- **Polya, George. *How to Solve It*. Princeton University Press, 1945.** The four-stage model: understand the problem → devise a plan → carry out the plan → look back. Problem formulation is stage 1, which Polya argued is the most underrated. Useful for one sentence in the chapter about how this is not a new insight, just newly urgent.
- **Simon, Herbert A. *The Sciences of the Artificial*, ch. 5 ("The Science of Design").** Design as problem formulation under uncertainty. The chapter's epistemological foundation.
- **Schön, Donald A. *The Reflective Practitioner*. Basic Books, 1983.** "Problem-setting" precedes problem-solving. Schön's argument that the practitioner sets problems through reflective conversation with the situation is the deep model under the `gh copilot ask` interrogation step.

### Key empirical cases

- **Mars Climate Orbiter loss (Sep 23, 1999).** Spacecraft lost because one team used pound-seconds for thrust, another used newton-seconds. Software ran correctly in both units. The *problem* — what units the system uses — was not formulated explicitly. NASA's MCO Mishap Investigation Board report (Nov 10, 1999) is open and well-documented. Killer detail: every individual calculation was correct. The unspecified problem was the cause.
- **The Y2K problem.** A formulation problem at industrial scale. The problem "we will store years as two digits" was right for the era it was specified in, wrong for the era it operated in. The chapter need not dwell on Y2K but can use it as a reference point for "the formulation that ages."
- **The Knight Capital flag-reuse incident (Ch 4, Ch 5).** Formulation failure: nobody asked "what is the new meaning of this flag, and where is the old meaning still in use?" The problem was not formulated; the symptom was the $440M loss.
- **Seth's "automating the wrong thing" moment (TIKTOC opening).** Thirty minutes into a build, Seth realizes he is automating the wrong task. Concrete worked example. Author should preserve Seth's specific case.

---

## 2. The Core Concept — State of the Field

### What is settled

- **Specification work is the largest single source of project cost overruns.** Established in software engineering (Boehm, Brooks, Standish Group CHAOS reports). The "find a bug" cost ratio between requirements-phase and post-release is 1:100 in classical estimates.
- **Problem formulation cannot be transferred to the tool.** The tool cannot know what the user *means* unless the user articulates it. This is not a model-capability question; it is a structural property of any tool whose input is the user's prompt.
- **Asking clarifying questions improves output quality.** Multiple papers in the prompt-engineering literature (2023–2025) show that systems that ask clarifying questions before generating produce better-matched outputs. The book's `gh copilot ask` interrogation step is in this lineage.

### What is disputed

- **Whether to formulate before or during.** Pure-Brooksian position: formulate completely before building. Iterative position: formulate enough to start, refine as you build. The book takes a hybrid stance — the *one-sentence formulation* is non-negotiable before suggest; refinement happens during. This is defensible and matches how experienced engineers actually work.
- **The level of detail in initial formulation.** TIKTOC specifies four elements (task statement, scope boundaries, exclusions, success definition). Some practitioners argue for more, some for less. The four-element minimum is defensible; the chapter should not insist on rigid completeness.
- **Whether `gh copilot ask` (or its successor) is the right tool for interrogation.** Some argue students should think alone first. The book's position is that interrogating with the model is itself a formulation practice — but the *student* must do the formulation work; the model is a sparring partner, not the formulator.

### What has changed recently (last 5 years)

- The 2024–2026 generation of agentic CLIs increasingly support a "plan" mode that produces a written plan before any commands run. This is institutional support for the chapter's argument. The book should acknowledge plan mode as the gate built into the tool while arguing that the *user must still formulate* before plan mode is invoked.
- Anthropic and OpenAI both released specific prompt-engineering guidance in 2024–2025 emphasizing problem decomposition. The chapter is in the mainstream of this guidance; it is not idiosyncratic.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 7 sits in shell script automation.)

- **"Archive my old projects" — formulated badly.** What does "old" mean? Modified date or creation date? Which directory? Move or compress? Where to? What if a name conflicts? The student who runs `gh copilot suggest "archive my old projects"` gets a generic answer to none of these. The student who formulates first gets a command tailored to actual intent.
- **"Set up a daily backup" — formulated badly.** Backup what to where, at what time, retained for how long, monitored how? Five questions; all answerable; none can be inferred from the prompt. Formulation surfaces all five.
- **"Clean up node_modules from old branches" — formulated badly.** Which branches? Local or all remotes? Just `node_modules` or also `.next`, `dist`, `build`? Hardest question: when something goes wrong, can you recover? Formulation forces the irreversibility question to the front of the build, not the end.

---

## 4. The Book's Thesis Connection

Ch 7 is where the supervisory discipline shifts from per-command to per-build. The gate (Ch 4) and the capacities (Ch 5) are command-level operations. CLI.md (Ch 6) persists across sessions. Problem formulation operates at the build level, before any session begins.

The chapter's contribution to the thesis:

1. **Most silent failures are formulation failures, not execution failures.** When a command exits 0 and does the wrong thing, the wrong thing was usually *specified* in the prompt — the model produced what was asked for. The problem was that what was asked for was not what the student needed.
2. **Formulation is irreducibly the student's work.** The model can clarify; the model can probe; the model cannot decide what the student *wants*. PF (Ch 5) gets its sharpest expression here.
3. **The cost of bad formulation compounds.** A weak formulation produces a weak prompt, which produces a generated command that matches the weak prompt, which exits 0 doing the wrong thing — and the student debugs the *command* when the issue was the *formulation* upstream.

Student-supplied capacity: the student is the only person who knows what they actually want. Formulation is the act of converting that into a sentence the model can act on.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Frederick Brooks (1931–2022).** Excellent fit. Keep as primary.

Candidates:

- **Frederick P. Brooks Jr.** (1931–2022, USA, computer scientist). The named figure. *The Mythical Man-Month*, "No Silver Bullet," *The Design of Design*. Brooks's career was an argument that the essential difficulty of software engineering is specification. Famous in software engineering, less famous to high schoolers in this specific frame (most know IBM/360 anecdotes). Diversity: white male American. Substantive fit is overwhelming.
- **Christopher Strachey** (1916–1975, UK, computer scientist). Co-author with Maurice Wilkes on early specification language work. Less famous than Brooks. Diversity: white male British. Marginal improvement on diversity (non-US).
- **Adele Goldberg** (born 1945, USA, computer scientist). Co-developer of Smalltalk. Goldberg's *Smalltalk-80* book is one of the great works on specification-as-design. Diversity: woman, American, late-20th-century. **Strongest diverse alternate.** Wikipedia-accessible.

Recommendation: **keep Brooks.** "No Silver Bullet" is the chapter's single best intellectual reference, and the substantive fit cannot be improved by swapping. Goldberg would be a fine alternate if rebalancing demands it; Strachey is too specialized.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
Ch 6's CLI.md. The reader should already be in the habit of writing down project context. Ch 7 extends this habit to per-build context.

### Common misconceptions to disarm
- **"I'll figure out the problem as I go."** Sometimes works for tiny tasks. Fails for anything that touches the filesystem in irreversible ways.
- **"My prompt was clear; the CLI got it wrong."** Usually the prompt was clear *to the student* but ambiguous in the model's training. Formulation forces the student to write a prompt that is clear to the model.
- **"Formulation is just rewording."** No. Formulation is decision work disguised as writing. What does "old" mean? — that is a decision, not a wording choice.
- **"The one-sentence test is too rigid."** It's a forcing function. If you cannot say what the script does, what it touches, and what it must never touch in one sentence, you have not finished formulation.

### Effective instructional sequences
- **Bad → better → best.** TIKTOC's pattern. Show the same task formulated three ways. The progression is the chapter's central pedagogical move.
- **The interrogation transcript.** Show a `gh copilot ask` interrogation in full — the questions, the model's clarifications, the student's revisions. Annotated. Five to ten exchanges. Shows what formulation actually looks like in practice.
- **The one-sentence test applied to a reader-supplied task.** The Apply-level exercise. The reader picks a task they want to automate, writes a one-sentence formulation, then revises it until the sentence passes the test (touches, doesn't touch, success criterion).

### Known failure modes
- **Formulation as bureaucratic ritual.** If formulation reads as paperwork, readers skip it. Frame it as the thirty-minute saver, not the thirty-minute tax. Seth's "I was automating the wrong thing" moment is the persuader.
- **Over-formulation.** Some readers will write half a page for a one-line script. The chapter should give a guideline: formulation should be roughly inversely proportional to reversibility. A `find -print` needs almost no formulation; an `rm -rf` deserves significant formulation.
- **Formulation theater.** Writing a one-sentence formulation without actually making the decisions it encodes. The chapter must distinguish formulation-as-decision-record from formulation-as-symbolic-compliance.

### What separates understanding from memorization
A reader who *understands* Ch 7 can take a task they have not yet built and walk through `gh copilot ask` interrogation, surfacing two or three decisions they had not consciously made. A reader who memorized Ch 7 can write a one-sentence formulation by following the template but cannot tell when their formulation is incomplete.

---

## 7. Representation and Display Research

TIKTOC specifies one figure:

- **`<!-- → [DIAGRAM: The problem formulation gate.] -->`** Worked content:

  Horizontal gate, with:
  - **Below the gate (allowed):** `gh copilot suggest` invocations.
  - **At the gate:** one sentence containing (1) what the script does, (2) what it touches, (3) what it must never touch.
  - **Above the gate (not yet allowed):** any suggest invocations attempting to act before formulation.
  - Editorial style; no color; visible "gate" affordance (a horizontal rule with a label).

The diagram is the chapter's mnemonic. Print-bookmarkable.

No additional displays required.

---

## 8. Open Questions and Research Gaps

- **`gh copilot ask` post-January-2026.** The retired `gh copilot ask` is replaced by the interactive `copilot` CLI's chat mode. The interrogation pattern survives; the surface changes. Update language at press.
- **Empirical formulation studies for AI-assisted work.** No published study cleanly measures formulation quality vs. AI-output quality for terminal tasks. The chapter's claim is consistent with general prompt-engineering findings but not directly measured. Acknowledge.
- **The one-sentence test as published artifact.** Author may want to print the one-sentence test on the back cover or inside cover as a usable reference card. The discipline lives or dies by how often readers consult it.
- **Formulation training transfer.** Will the discipline transfer to non-terminal AI work (writing, editor coding)? Likely yes; not measured. Out of scope for this book.

---

## 9. Sourcing Notes

- **Brooks 1987 "No Silver Bullet"** — open access via IEEE archive or Brooks's UNC page.
- **Brooks 2010** *The Design of Design* — Addison-Wesley, standard edition.
- **Polya 1945** — Princeton University Press classic; many editions.
- **Schön 1983** — Basic Books, standard citation.
- **NASA MCO Mishap Investigation Board report (Nov 10, 1999)** — open access via nasa.gov; stable URL.
- **Anthropic / OpenAI prompt-engineering guidance**: cite the version current at press; these evolve.
