# Research: Chapter 11 — Planning Your First Conducted Build
## GitHub Copilot CLI for Students: A Practitioner's Guide

**Chapter one-line:** Before `gh copilot suggest` runs a single command, you know exactly what you are building, why, and which steps belong to you.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Alexander, Christopher. *Notes on the Synthesis of Form*. Harvard University Press, 1964.** Alexander's argument that design begins with a clear statement of "fit" between form and context. The chapter borrows the foundation: planning is the act of making the fit explicit before construction begins. Less famous than *A Pattern Language* but more directly relevant.
- **Alexander, Christopher et al. *A Pattern Language*. Oxford, 1977.** Patterns as reusable solutions to recurring problems. Useful for the chapter's frame that planning is about identifying which patterns from prior builds apply to this one.
- **Brooks, Frederick P. *The Mythical Man-Month*, ch. 5 ("The Second-System Effect"). Addison-Wesley, 1975 / 1995.** Brooks's argument that the most dangerous design is the one made without restraint — a planner who has acquired confidence from a previous build can over-engineer. Useful counterpoint: the chapter must keep first-build plans appropriately scoped.
- **Knuth, Donald E. "Structured Programming with go to Statements." *ACM Computing Surveys* 6, no. 4 (1974).** Knuth's "premature optimization is the root of all evil" quote belongs to first-build planning. Plan for clarity first; optimize later.
- **Beck, Kent. *Extreme Programming Explained*, 2nd ed. Addison-Wesley, 2004.** XP's planning game: the planner identifies smallest viable units of work and orders them. The chapter's "minimum viable plan" is XP-flavored: enough to start, refined as you build.

### Key empirical cases

- **The Apollo 11 mission planning record (NASA archives).** Margaret Hamilton's flight-software team produced explicit, written plans before code: requirements, edge cases, fault-handling sequences. The chapter can use this as a precedent — planning at the strict end of the spectrum — without conscripting the reader to the same level. Hamilton's plans are public via NASA's Apollo archives.
- **The Standish Group CHAOS reports (1994–present).** Decades of evidence that planning quality correlates strongly with project success. The 2020s reports specifically address agile vs. waterfall framings, but the consistent finding is that *some* upfront planning, however lightweight, beats none.
- **The "Friday afternoon deploy" pattern.** Recurring failure mode: a build run without explicit planning, on a Friday afternoon, with no plan for what "done" looks like. Documented widely in DevOps post-mortems (Etsy, Netflix engineering blogs 2015–2024). The chapter can reference this as the cultural anti-pattern.

---

## 2. The Core Concept — State of the Field

### What is settled

- **Plans reduce error rates.** Established broadly in software engineering and human-factors literature. The mechanism: planning makes implicit decisions explicit, surfacing them for review.
- **Planning quality is bimodal.** Either you have *a* plan or you don't. The difference between a one-paragraph plan and a thirty-page plan is smaller than the difference between *any* plan and no plan. The chapter's "minimum viable plan" leans on this finding.
- **Planning is a learnable skill.** Not a personality trait. The chapter assumes this and structures itself as instruction in a skill.

### What is disputed

- **Waterfall vs. iterative.** Decades-long debate. The chapter sidesteps by recommending a *minimum* upfront plan that is refined during the build. This is the consensus practical position; it does not require staking out a side.
- **Plan as document vs. plan as conversation.** Some practitioners argue plans should be written; others argue they should be spoken aloud with collaborators. The book recommends written, because the reader is often working alone and the plan is also the artifact that informs CLI.md.
- **The role of agentic plan-mode in CLI tools.** The post-January-2026 `copilot` CLI plan mode generates plans on the user's behalf. Some practitioners delegate planning to the tool entirely. The book's position: the *student* plans first, then asks the tool to plan, then reconciles. The tool's plan is a check on the student's plan, not a replacement.

### What has changed recently (last 5 years)

- The widespread availability of plan-mode in CLI tools (2025–2026) has changed planning culture for AI-assisted work. The chapter is teaching the discipline that exploits plan-mode without surrendering to it.
- Increased empirical work on AI-assisted planning quality (Anthropic's 2025–2026 research) suggests that students who pre-plan and then engage with AI-generated plans produce higher-quality builds than either pure-human planning or pure-AI planning. The chapter's hybrid stance is empirically supported.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 11 sits in shell script automation per TIKTOC.)

- **A homework-archive build.** Plan inputs: `~/homework/2025-fall/` exists; archive destination `~/archive/2025-fall/`; success means all files moved, none lost, no `.git/` directories accidentally moved. Plan outputs: three steps — survey, dry-run move, real move — with handoff conditions for each.
- **A daily-backup script.** Plan inputs: `~/projects/` is the source; `~/backups/` is the destination; success means daily rotation with 7-day retention. Plan outputs: cron job, backup script, retention script, monitoring approach.
- **A development-environment setup script.** Plan inputs: a fresh machine needs `~/.zshrc`, the student's preferred CLI tools, GitHub authentication. Success means a new machine reaches the student's familiar state in under 10 minutes. Plan outputs: idempotent script, list of preferences, failure-recovery steps.

---

## 4. The Book's Thesis Connection

Ch 11 begins Act Three by shifting the reader from "framework learned" to "framework applied." The thesis ("the student who conducts builds capability") requires the student to *initiate* a build using the framework. Planning is the first chapter where the student leads.

The chapter's contribution:

1. **Plans operationalize the supervisory capacities.** Every plan element exercises a capacity. Problem statement → PF. Scope decisions → IJ. Step ordering → EI. Failure-handling → PA. Tool choices → TO. The chapter can make this map explicit at the end.
2. **Plans bridge formulation (Ch 7) and execution (Ch 12).** Formulation produced a one-sentence statement; planning expands it into a sequence of steps with handoff conditions. The build is the plan's execution.
3. **First plans are practice.** The chapter must frame planning as a skill that improves with repetition. The first plan will be imperfect; the discipline is in producing it, reviewing it, executing it, and learning from the gap between plan and outcome.

Student-supplied capacity: every plan decision requires knowledge the model lacks — about the student's environment, intent, and tolerance for cost. Plans are intent made explicit.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Christopher Alexander (1936–2022).** Strong fit. Keep as primary.

Candidates:

- **Christopher Alexander** (1936–2022, Austria/UK/USA, architect). The named figure. *Notes on the Synthesis of Form*, *A Pattern Language*, *The Timeless Way of Building*. Alexander's career was about making design intent explicit before building. Diversity: Austrian-born, multinational career, white male, mid-20th to early-21st-century. Wikipedia-accessible. Substantive fit is strong.
- **Phyllis Pearsall** (1906–1996, UK, cartographer). Pearsall walked 3,000 miles cataloging London streets to create the *A–Z London Atlas* (1936). Planning at the level of "I will personally walk every street." Less famous; substantive fit is moderate (the act of advance reconnaissance is the connection). Diversity: woman, British, mid-20th-century.
- **Maria Telkes** (1900–1995, Hungary/USA, scientist). Solar-energy engineer who planned the first solar-heated house (Dover Sun House, 1948) with explicit per-component performance specifications. Diversity: woman, Hungarian-American. Lesser-known. Substantive fit is moderate.

Recommendation: **keep Alexander.** *Notes on the Synthesis of Form* is the cleanest intellectual ancestor for "design begins with explicit statement of the problem." Pearsall is a charming alternate but the fit is more poetic than direct. Telkes is interesting but the substantive distance is greater.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
The full Act Two framework. The chapter assumes the reader can write a formulation, a CLI.md, a specification-shaped prompt, and a handoff condition. Ch 11 composes them into a plan.

### Common misconceptions to disarm
- **"Planning is for big builds."** First-builds especially benefit. The reader's first plan is a learning artifact — the gap between the plan and the outcome teaches more than the build itself.
- **"My plan should be exhaustive."** No. The minimum-viable plan is the goal. Detail in proportion to consequence.
- **"I'll plan as I go."** The chapter must counter this. Planning during execution mixes formulation work with execution work; both suffer.
- **"`gh copilot ask` will plan it for me."** It can produce a plan. The student must produce *their* plan first, then reconcile with the tool's.

### Effective instructional sequences
- **A full worked plan, in Seth's voice.** The chapter's spine. Show one complete plan for one real Seth task. Annotate each section with which capacity is exercised.
- **The minimum-viable plan template.** Six elements: one-sentence task, success criterion, step list, handoff conditions per step, tool choices per step, failure-recovery approach. Show the template; show Seth filling it in.
- **`gh copilot ask` reconciliation.** Show Seth asking the tool to plan the same task, then showing the differences, then deciding which differences are worth adopting and which to override.

### Known failure modes
- **The plan as ritual.** Readers will produce structurally complete plans with semantically empty content. The chapter must emphasize *decision-making* over *form-filling*.
- **Over-planning a first build.** A first plan should not exceed one page. If it does, the build is too big; pick a smaller one.
- **Surrender to plan-mode.** Readers may default to letting the tool plan. The chapter must defend the *student plans first* discipline.

### What separates understanding from memorization
A reader who *understands* Ch 11 can take a new task and produce a one-page plan that names success criteria, scopes the work, and identifies the riskiest steps. A reader who memorized Ch 11 can fill in the template without producing decisions worth executing.

---

## 7. Representation and Display Research

TIKTOC specifies one figure:

- **`<!-- → [DIAGRAM: The planning sequence.] -->`** Worked content:

  Horizontal flow with phase gates marked:
  `gh copilot ask interrogation` → `problem formulation` → `CLI.md populated` → `execution plan` → `review and approve` → [GATE: nothing crosses without review] → `gh copilot suggest invocations`.

  Editorial style. Each phase is a box; the gate between review and suggest is the chapter's emphasized affordance.

No additional displays required. (A second optional artifact: the minimum-viable plan template as a printable card. Reader-aid; not required by TIKTOC.)

---

## 8. Open Questions and Research Gaps

- **Seth's specific build project.** TIKTOC's OQ-1 calls this out. The author and Seth must select and document a concrete project before drafting Ch 11–14. The chapter's quality depends on this case being *real* and *reproducible*.
- **Plan-mode interaction in the `copilot` CLI.** Author should test the new CLI's plan mode against a Seth task and document the experience. Will inform the chapter's specific guidance on plan-mode reconciliation.
- **Empirical first-plan quality measurement.** No published study measures the quality of student first-plans before vs. after framework instruction. Author may want to pilot this with Seth and a cohort.

---

## 9. Sourcing Notes

- **Alexander 1964** *Notes on the Synthesis of Form* — Harvard University Press; standard edition.
- **Alexander 1977** *A Pattern Language* — Oxford University Press; widely used.
- **Brooks 1995** — anniversary edition includes a 20th-anniversary preface.
- **Knuth 1974** — open access via ACM archive.
- **Beck 2004** — Addison-Wesley.
- **Apollo planning records**: NASA Technical Reports Server (ntrs.nasa.gov). Open access.
- **Standish CHAOS reports**: proprietary but widely summarized. Cite a recent (2023+) summary if used.
