# Research: Chapter 13 — Verification: How You Know It Works
## GitHub Copilot CLI for Students: A Practitioner's Guide

**Chapter one-line:** The build is done when it passes the handoff conditions — not when `gh copilot suggest` says it's done, not when it exits 0.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Liskov, Barbara H. and Jeannette M. Wing. "A Behavioral Notion of Subtyping." *ACM TOPLAS* 16, no. 6 (1994): 1811–1841.** Liskov's contribution: a behavioral contract (preconditions, postconditions, invariants) must be defined before "correct" has meaning. The chapter's intent-verification pass is Liskov's "behavioral correctness" applied to one-off terminal builds.
- **Hoare, C.A.R. "An Axiomatic Basis for Computer Programming." *CACM* 12, no. 10 (1969): 576–580.** Hoare triples: {precondition} command {postcondition}. The chapter's three-pass verification (mechanical / scope / intent) is three layers of Hoare-style postcondition checking.
- **Leveson, Nancy G. *Engineering a Safer World*. MIT Press, 2011.** Particularly STAMP. Verification is not just checking that something happened; it's checking that the *controlled variable* is in the intended state. The chapter's intent pass is STAMP's "is the system in the state we wanted?" question.
- **Argyris, Chris and Donald A. Schön. *Organizational Learning II: Theory, Method, and Practice*. Addison-Wesley, 1996.** Double-loop learning: not just "did the action work?" but "was the goal the right goal?" The post-build learning document is double-loop learning for a single build.
- **Boehm, Barry. "A Spiral Model of Software Development and Enhancement." *Computer* 21, no. 5 (1988): 61–72.** Boehm's spiral model alternates construction with risk-driven verification. The chapter's three-pass verification is a single-build spiral.

### Key empirical cases

- **NASA verification practice (Apollo onwards).** Hamilton's flight software underwent layered verification: unit, integration, mission-simulation, mission. The chapter need not match the rigor but can borrow the *layering* principle: verify at multiple levels, in sequence, with different criteria each pass.
- **The "everything green, system still wrong" pattern.** Recurring failure: every monitoring dashboard reports healthy, but the actual system delivers wrong results. Documented widely in SRE literature. The chapter's mechanical-vs-intent distinction names this pattern.
- **Seth's near-miss verification (TIKTOC opening).** Seth's build exits 0 across every step. He runs the verification pass he almost skipped. He finds a silent scope error. Concrete worked example required from author.
- **The dry-run-as-production-test pattern.** Common professional practice: run the script with `--dry-run` against the *real* dataset (not a synthetic one) and inspect the output before committing to the real execution. The chapter should teach this explicitly.

---

## 2. The Core Concept — State of the Field

### What is settled

- **Verification is layered, not monolithic.** Established in software engineering (Boehm), aerospace (NASA), and safety-critical industries. The three-pass sequence (mechanical / scope / intent) is a defensible compact version of standard practice.
- **Exit codes ≠ correctness ≠ intent satisfaction.** Three distinct things; the chapter's job is to keep them distinct.
- **Verification quality depends on what was specified.** A build can only be verified against the criteria the student set. Unset criteria are unverifiable. The chapter's recursive point: the verification pass is only as good as Ch 7's formulation and Ch 9's handoff conditions.

### What is disputed

- **The right number of passes.** Some practitioners use two (mechanical + intent). Some use four or five (adding security, performance, ethics). The book's three (mechanical / scope / intent) is a defensible operational compact.
- **Whether intent verification can be partially automated.** Some practitioners argue automated test suites cover intent. The chapter's position: tests cover *specified* intent; *unspecified* intent (the things you didn't think to test) is what the dangerous middle exploits. Manual intent verification is irreducible for ad-hoc terminal work.
- **Whether the post-build document is necessary or ceremonial.** Some practitioners view explicit post-build review as overhead. The book's position: for *learning* builds, the document is the learning artifact. For production builds at scale, the equivalent is the post-mortem. Both serve the same function.

### What has changed recently (last 5 years)

- The 2025–2026 generation of CLIs supports automated verification scripts (e.g., `--verify-against=expected.json`). Useful when the expected state is specifiable. The chapter should mention this without elevating it above human intent verification.
- The growing recognition (post-2023) that "the AI confirmed it worked" is not verification has produced industry guidance on independent verification. The chapter is teaching the discipline the industry is now naming.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 13 sits in the student shell project per TIKTOC.)

- **Mechanical pass for a homework archive.** Did the script exit 0? Were any errors reported in stderr? Did it complete in expected time? Yes / yes / yes — proceed to next pass.
- **Scope pass for the same build.** How many directories were moved? Are they the ones the survey identified in Ch 12 Step 1? Are *any* directories outside the source directory affected? Are any directories that should have been excluded actually present in destination? Count match, identity match, exclusion check. Pass.
- **Intent pass.** Does the result match what the formulation said the build would produce? Specifically: is the source dir empty of last semester's content? Is the archive dir's structure usable for future reference? Did the build avoid touching anything not named in the formulation? Pass.

Three passes; three different questions; three different methods.

---

## 4. The Book's Thesis Connection

Ch 13 is the chapter that defines "done." If Ch 1's claim is "exit 0 ≠ correct," Ch 13 is the positive operational answer: a build is done when it passes mechanical, scope, and intent verification — and the student has produced a post-build document recording what was learned.

The chapter's contribution:

1. **Three-pass discipline.** Operational, memorable, repeatable. The reader can apply it to every build for the rest of their life.
2. **Post-build document as learning artifact.** The thesis is about capability. The post-build document is the proof — the build that produced a document where the student can articulate every decision is the build that produced capability.
3. **The student is the verifier.** The CLI cannot verify intent. The chapter must defend this against the "but automated tests" objection — automated tests are *codified* intent; the student must still decide whether the codification was right.

Student-supplied capacity: intent is held only by the student. Intent-verification is the act of confirming reality against intent. The chapter is the discipline of that confirmation.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Barbara Liskov (born 1939).** Strong fit. Keep as primary.

Candidates:

- **Barbara Liskov** (born 1939, USA, computer scientist). The named figure. Turing Award (2008). Behavioral subtyping, Liskov Substitution Principle. Liskov's career was about making "correct behavior" formally specifiable. Diversity: woman, American, late-20th to 21st-century. Wikipedia-accessible. **Excellent choice.** Helps diversity spread.
- **Nancy Leveson** (born 1948, USA, computer scientist/safety engineer). STAMP, *Safeware*, *Engineering a Safer World*. Leveson's argument that verification must check the controlled variable, not just the action, is the chapter's intent pass made formal. Diversity: woman, American. Strong alternate.
- **Cliff Stoll** (born 1950, USA, astronomer/system administrator). *The Cuckoo's Egg* (1989). Stoll's painstaking verification of system state to track an intruder is verification in narrative form. Less academic; great storyteller. Diversity: white male American. Substantive fit is moderate; great voice for sidebar.

Recommendation: **keep Liskov.** Behavioral correctness is the chapter's intellectual core, and Liskov is the foundational figure. Leveson is also excellent; either is a strong primary. Stoll is a great sidebar reference but not the right Wayback Machine figure for this chapter.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
The full framework. Ch 13 verifies what Ch 1–12 built.

### Common misconceptions to disarm
- **"Exit 0 means it worked."** The book's founding misconception. Re-disarmed here in concrete operational form.
- **"If the build ran, it's done."** No. The build is done when it passes the verification pass. Running is necessary, not sufficient.
- **"Intent verification is just gut feeling."** No. Intent verification is comparison against a *written* artifact (the formulation, the handoff conditions). Without the written artifact, intent verification reduces to gut feeling — and gut feeling has well-known failure modes (confirmation bias, sunk-cost reluctance to revert).
- **"The post-build document is for big builds only."** No. The first build especially benefits. The student's *first* post-build document is the artifact that proves the framework worked.

### Effective instructional sequences
- **Seth's near-miss as opener.** TIKTOC's opening. Concrete; emotionally engaging; teaches the lesson by example.
- **Three passes in sequence.** Show what each pass looks like for Seth's build. Mechanical: 30 seconds. Scope: 2 minutes. Intent: 5 minutes. The chapter's tempo is the pass cadence.
- **Post-build document template.** TIKTOC's five sections: what I built / what I delegated and why / what I kept and why / what I learned / what I would do differently. Show Seth's filled-in version.
- **The reader's first post-build.** Apply-level exercise. Have the reader complete their own post-build document for a build they have already attempted.

### Known failure modes
- **The chapter as test-suite advocacy.** Verification is not just running tests. The chapter must not collapse into "write more tests."
- **Verification fatigue.** Three passes feels like a lot. The chapter must show that mechanical and scope passes are fast; intent is the slow one. Total verification time is often less than the build time.
- **Post-build document as paperwork.** If readers experience the document as bureaucratic, they skip it. Frame it as a thinking-tool, not a record-keeping tool.

### What separates understanding from memorization
A reader who *understands* Ch 13 can take a build they have done in the past and produce a retrospective post-build document that names a real decision they would now reverse. A reader who memorized Ch 13 can fill in the document template without producing self-criticism worth acting on.

---

## 7. Representation and Display Research

TIKTOC specifies one figure:

- **`<!-- → [DIAGRAM: The verification sequence — three passes.] -->`** Worked content:

  Horizontal sequence: `Mechanical pass [exit 0, expected output, no errors] → Scope pass [right files, right directories, right count] → Intent pass [does the result match formulation?]`. Each pass labeled as binary pass/fail. Editorial style. Each pass shown as a checkpoint affordance (horizontal rule with check icon).

No additional displays required. (Optional: the post-build document template as a printable card.)

---

## 8. Open Questions and Research Gaps

- **The "almost skipped it" moment.** TIKTOC's opening requires a real Seth case. Author should preserve the specific facts (which command, what slipped past mechanical/scope, what intent verification caught).
- **Automated verification integration.** As more CLIs ship `--verify-against` capabilities, the chapter should address how automated verification fits with the three passes. Recommendation: automated verification is a tool for the scope pass; the intent pass remains human.
- **Post-build document evolution.** As the student matures, the document's content may change. Author may want to show a Seth post-build at build 1, build 10, build 50 — the document becomes shorter but sharper as the framework internalizes.

---

## 9. Sourcing Notes

- **Liskov & Wing 1994** — ACM Digital Library.
- **Hoare 1969** — open access via CACM archive.
- **Leveson 2011** — MIT Press; some open access chapters.
- **Argyris & Schön 1996** — Addison-Wesley, foundational text in organizational learning.
- **Boehm 1988** — open access via IEEE archive.
- **Stoll 1989** *The Cuckoo's Egg* — Doubleday, trade book, widely available.
