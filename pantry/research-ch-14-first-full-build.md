# Research: Chapter 14 — Your First Full Build: From Problem to Verified Output
## GitHub Copilot CLI for Students: A Practitioner's Guide

**Chapter one-line:** You have the discipline. Here is the project. Conduct it.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Dewey, John. *Experience and Education*. Macmillan, 1938.** Dewey's argument: learning is not the transmission of information but the transformation of the learner through purposeful experience. The chapter's claim that the post-build document is the record of transformation is Dewey applied to terminal AI.
- **Dewey, John. *How We Think*. D.C. Heath, 1910 (rev. 1933).** The reflective-thought cycle: doubt → analysis → hypothesis → testing → revision. Each step of the conducting framework is a Deweyan move. The chapter can borrow the cycle without naming it.
- **Schön, Donald A. *The Reflective Practitioner*. Basic Books, 1983.** Reflection-in-action and reflection-on-action. Ch 14 is reflection-on-action: the post-build document is the artifact in which reflection becomes durable.
- **Papert, Seymour. *Mindstorms*. Basic Books, 1980.** Papert's argument that students should build artifacts they personally find meaningful. The chapter's project brief should be one the *reader* finds genuinely useful, not a contrived exercise.
- **Brown, John Seely; Allan Collins; Paul Duguid. "Situated Cognition and the Culture of Learning." *Educational Researcher* 18, no. 1 (1989): 32–42.** Cognition is situated in practice; capability transfers through actual situated work, not through abstract instruction. The chapter is the situated practice that consummates the abstract Acts One and Two.

### Key empirical cases

- **The chapter's central case is the reader's own build.** Not Seth's. The chapter must provide a project brief detailed enough that the reader can execute it, generic enough that it fits varied student environments. Likely a file-management or repo-management task; specific enough to require the framework, simple enough to complete in a sitting.
- **Variations across reader environments.** Author should anticipate: the reader using macOS, Linux (WSL or native), or Windows PowerShell. The project brief should specify the operating constraint explicitly or provide adaptable elements.
- **Successful student-completion cases.** If the author can pilot the chapter with five students before publication, the chapter benefits enormously. Their post-build documents are the proof.

---

## 2. The Core Concept — State of the Field

### What is settled

- **Hands-on application of a framework consolidates the framework.** Established in education (Dewey, Bruner, Papert) and skill-acquisition research (Ericsson, K. Anders). Reading about conducting does not produce conducting. Doing produces conducting.
- **First independent applications are the bottleneck.** The transition from "I can do this with help" to "I can do this alone" is the steepest part of the learning curve. The chapter is the bridge.
- **The post-build document as portable evidence.** A completed post-build document is something the student can show — to themselves, to a teacher, to an interviewer — as evidence of capability. The artifact is real.

### What is disputed

- **How structured the chapter should be.** Some pedagogies argue the final chapter should hand the student a fully open-ended project ("here are the tools; build something"). Others argue for a structured first build with one degree of freedom released at a time. The book's position (TIKTOC): structured brief, full framework application, the student decides everything within the brief. Defensible middle position.
- **Whether to grade or evaluate.** The book is self-directed; there is no grade. The post-build document is self-evaluation. Some pedagogies argue external evaluation should still appear. The book's stance: the reader is the evaluator; the framework is the rubric.
- **The next-build question.** Some readers will finish the chapter and ask "what now?" The chapter could close with a "next-build" pathway. TIKTOC does not require this but the author may want to add a closing section pointing to the teacher-companion book, the Claude Code / Codex series books, or independent practice.

### What has changed recently (last 5 years)

- The post-January-2026 `copilot` CLI changes the specific tool commands but not the chapter's pedagogy. The chapter's brief can be written tool-agnostically: "use the CLI's natural-language command generator" rather than "use `gh copilot suggest`."
- Growing institutional adoption of capstone-project models in CS education (2024–2026). The chapter's first-full-build is in this lineage — capability demonstrated by a completed, documented artifact.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 14 is the full-build chapter — the reader's own project.)

Candidate project briefs (author chooses one or offers a small menu):

- **The repository hygiene script.** Goal: a script that cleans up a student's project repositories — removes `node_modules/`, `dist/`, and other regenerable artifacts from non-active branches, preserving the active branch's working state. Touches: file deletion (irreversible), Git state (potentially destructive). All five capacities required. Plausibility auditing critical. Concrete success criterion: a measurable disk-space reduction with no loss of active work.
- **The daily-summary builder.** Goal: a script that summarizes the day's Git commits, file changes, and notes into a single end-of-day document. Touches: read-only file operations on the student's home directory. Lower irreversibility, more creative judgment (Ch 10 territory). Good for students who prefer report-generation work.
- **The development-environment bootstrapper.** Goal: a script that sets up a new computer (or a new VM) with the student's preferred dotfiles, CLI tools, and Git config. Touches: writes to home directory, modifies shell config. Idempotency is required. Good for students who want a tool they will reuse.

The chapter should offer two or three; the author should pick the one most likely to fit a wide reader base. The repository hygiene script is the strongest single choice — it touches the dangerous middle (deletion + Git), exercises all five capacities, and produces a tool the reader will keep.

---

## 4. The Book's Thesis Connection

Ch 14 is where the thesis is consummated. Every prior chapter was preparation; Ch 14 is the demonstration. The reader does what the book has been teaching. The post-build document is evidence that the discipline worked.

The chapter's contribution:

1. **The transfer.** Capability built through Seth's narrative now belongs to the reader. The chapter's job is the transfer, performed.
2. **The artifact.** The post-build document is a concrete deliverable. It exists. The reader can show it. The book has produced something measurable.
3. **The arc closes on the reader.** TIKTOC's stated structural goal. Seth's story was preparation; the reader's build is the consummation. The chapter's final paragraphs hand the practice forward.

Student-supplied capacity: everything. The chapter is the reader operating as a conductor without scaffolding. The book has made itself unnecessary.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: John Dewey (1859–1952).** Strong fit. Keep as primary.

Candidates:

- **John Dewey** (1859–1952, USA, philosopher/educator). The named figure. *Experience and Education*, *How We Think*, *Democracy and Education*. Dewey's argument — learning is transformation through purposeful experience — is the book's pedagogical foundation. Famous to educators; less to high schoolers. Diversity: white male American, early-20th-century.
- **Paulo Freire** (1921–1997, Brazil, educator). Already noted as a strong candidate for Ch 3. Freire's praxis (action + reflection) is exactly the post-build document discipline. Diversity: Brazilian. If the author swapped Freire into Ch 3, Dewey here is fine. If the author kept Illich in Ch 3, Freire could swap in here.
- **Charlotte Mason** (1842–1923, UK, educator). The "Charlotte Mason method" emphasized first-hand experience and student self-direction. Less famous than Dewey, similar period, different national context. Diversity: woman, British, late-19th to early-20th-century. Wikipedia-accessible.

Recommendation: **keep Dewey.** *Experience and Education* is the book's pedagogical anchor; using Dewey here makes that lineage visible. If the author chose Freire for Ch 3, Dewey here completes a Dewey-Freire bracket that frames the book's pedagogy without overlap. If the author kept Illich in Ch 3, Dewey here still works; Freire could be a fine alternate.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
Everything. Ch 14 is the capstone.

### Common misconceptions to disarm
- **"The framework is too elaborate for a single small build."** No. The framework scales. A 30-minute build uses all five capacities; they fire faster, not differently.
- **"I'll skip the framework on the easy parts."** The easy parts are where the dangerous middle lives. The framework applies most strongly where the temptation to skip it is highest.
- **"The post-build document is for the framework, not for me."** No. The document is for the reader's future self. The reader is the only person who benefits from the document. The framework is the form; the content is the reader's own learning.
- **"I'm not ready."** The chapter must counter this directly. Readiness is not the threshold; *practice* is the threshold. The first build will not be perfect. That's the point.

### Effective instructional sequences
- **Project brief in detail.** TIKTOC specifies: project brief, tools, sequence. The chapter must provide these concretely.
- **The student's hand on every decision.** The chapter must resist over-helping. Each section can name *what* the student must do, not *how*. The how is the framework, already taught.
- **The closing paragraph.** TIKTOC supplies one already: "You built it. You can explain every command that ran…" Use this or close to it. The voice should be the reader's friend, not the teacher.

### Known failure modes
- **The chapter as homework.** If Ch 14 reads as an assignment, readers stall. Frame the project as a gift — a small useful tool they'll keep, built in their own hands.
- **The "your build was wrong" experience.** Readers will produce imperfect builds. The chapter must pre-equip them: imperfect first builds are normal; the post-build document captures the imperfection, names it, and turns it into the next build's specification.
- **The book's voice continuing too long.** The book ends here. The reader is now the practitioner. The chapter must not over-stay.

### What separates understanding from memorization
A reader who *understands* Ch 14 completes their first build and produces a post-build document that contains a specific, named insight they did not have at the start of the book. A reader who memorized Ch 14 produces a build whose post-build document is generic and recyclable across any reader.

---

## 7. Representation and Display Research

TIKTOC specifies one figure:

- **`<!-- → [DIAGRAM: The complete arc — four milestones.] -->`** Worked content:

  Horizontal four-point timeline:
  `Problem named (Ch 1)` → `Discipline learned (Ch 4–10)` → `First build planned (Ch 11)` → `First build verified (Ch 13–14)`.

  Editorial style. The chapter's closing image. Should feel like a punctuation mark — a stage marker, not a flowchart.

No additional displays required. The chapter's energy is on the reader's build, not on diagrams.

---

## 8. Open Questions and Research Gaps

- **The chapter's central project brief.** Recommendation: repository-hygiene script. Author and Seth should pilot together before drafting.
- **Closing-section question.** Should Ch 14 close with pointers to next builds, the teacher-companion book, or the rest of the series? TIKTOC does not require this. Author's call. Recommendation: a brief closing section ("if you want to keep going…") with three pointers — one to a next-build idea, one to the teacher companion, one to the Claude Code or Codex sibling book.
- **The book's post-publication evolution.** The Wayback Machine figures, the version notes, and the specific Seth cases will need refresh. Author should plan for a 2-edition cadence: edition 1 in 2026, edition 2 in 2028–2029 once the post-January-2026 CLI surface stabilizes.

---

## 9. Sourcing Notes

- **Dewey 1938** *Experience and Education* — Kappa Delta Pi reprint (1998) is the standard recent edition.
- **Dewey 1910 / 1933** *How We Think* — many reprints; the 1933 revision is the standard.
- **Schön 1983** *The Reflective Practitioner* — Basic Books, standard edition.
- **Papert 1980** *Mindstorms* — Basic Books; 2nd edition (1993) has a useful new preface.
- **Brown, Collins & Duguid 1989** — open access via *Educational Researcher* archives or AERA.
