# Research: Chapter 12 — Running the Build: CLI Tasks and Human Tasks
## GitHub Copilot CLI for Students: A Practitioner's Guide

**Chapter one-line:** The plan is approved. Now you execute it — one command at a time, with the suggest → explain → verify gate applied at every step.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Deming, W. Edwards. *Out of the Crisis*, ch. 1 ("The 14 Points") and ch. 2 ("Plan-Do-Check-Act"). MIT Press, 1986.** Deming's cycle is the chapter's operational model. Each command in a conducted build is a PDCA loop: plan the command (Ch 11), do (suggest → explain → run), check (handoff condition), act (revert and respecify if check fails, or continue if passes).
- **Spear, Steven J. and H. Kent Bowen. "Decoding the DNA of the Toyota Production System." *Harvard Business Review* (Sep 1999).** Toyota's principle that every step has explicit specifications, and that work stops when a step fails its specification. The "andon cord" pulled at any abnormality. The chapter's "revert and respecify" is the andon-cord discipline applied to terminal builds.
- **Allspaw, John and Jesse Robbins, eds. *Web Operations: Keeping the Data on Time*. O'Reilly, 2010.** Particularly chapters on incident response. The book's operational vocabulary (alerting, escalation, post-incident review) is the lineage under "stop the build when something feels off."
- **Beyer, Betsy et al. *Site Reliability Engineering* (Google SRE book). O'Reilly, 2016, ch. 13 ("Emergency Response") and ch. 14 ("Managing Incidents").** The discipline of stopping, naming, communicating, and resolving incidents. Useful for the chapter's framing of "scope creep" as an incident to address explicitly rather than absorb.
- **Anthropic RCT (Ch 1).** Specifically the "iterative AI debugging" low-scoring pattern. The chapter is teaching the discipline that prevents readers from sliding into iterative-debugging mode when a command fails its handoff condition.

### Key empirical cases

- **The Etsy "blameless post-mortem" practice (mid-2010s).** Etsy's engineering blog documented their practice of post-incident review that names what went wrong without blaming individuals. The chapter's "revert and respecify, then update CLI.md" is the personal-scale version of blameless post-mortem.
- **The "two-failed-corrections" rule applied to a Seth build.** Author's concrete worked example required: a Seth session where two consecutive `gh copilot suggest` outputs failed handoff conditions. The book's prescription: restate the problem from scratch with a better specification rather than continue correcting. Show this play out.
- **The "while we're here" scope creep pattern.** Recurring failure mode: the CLI proposes an unrelated improvement during a build. The student accepts. The build now does two things. Either could fail; both might. Documented broadly in DevOps culture (Etsy, GitLab, Netflix post-mortems). The chapter's rule — add to CLI.md, do not execute now — is the discipline.

---

## 2. The Core Concept — State of the Field

### What is settled

- **Stopping work at failure points reduces compounding error.** Established in manufacturing (Toyota), software (SRE practice), and aviation (Cockpit Resource Management). The chapter's revert-and-respecify rule is in a 70-year lineage of stop-on-failure discipline.
- **Scope creep during execution increases project cost and error rates.** Standard finding in software engineering and operations literature. The chapter's prescription matches consensus practice.
- **Post-execution review (post-build documents, post-mortems) improves future performance.** Established in human-factors (Argyris and Schön's "double-loop learning") and aviation (NTSB post-accident reviews). The chapter's post-build document is in this lineage.

### What is disputed

- **Whether "revert and respecify" is appropriate for student-scale work.** Some practitioners argue the rule is overkill for low-stakes student tasks. The book's position: the discipline is most valuable when learned at low stakes, so it becomes automatic at higher stakes. Defensible.
- **How long to attempt correction before reverting.** TIKTOC says two failed corrections, then restate. Some practitioners argue three; some argue one. Two is a defensible compromise; the book should explain the rationale (more attempts indicate the formulation is wrong, not the command).
- **Whether to use plan-mode for execution.** The post-January-2026 `copilot` CLI plan mode handles the chapter's "suggest → explain → run, one step at a time" sequence as a built-in feature. Some practitioners delegate execution to plan mode entirely. The book's position: plan-mode is a tool to use; the student must still actively review each step. This is the dangerous-middle warning applied to execution.

### What has changed recently (last 5 years)

- The 2026 generation of agentic CLIs can chain operations more autonomously than the 2023 generation. This raises stakes for the *active review* discipline — the agent will keep going unless the user intervenes. The chapter must address this directly.
- The widespread adoption of "approval-required" flags for destructive operations (e.g., `--require-approval` patterns) in 2024–2026 enterprise tools is institutional support for the chapter's per-step discipline.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 12 sits in the student shell project — Seth's actual full build.)

The chapter's worked example must be Seth's real build, executed step by step. Suggested structure:

- **Step 1:** Survey the source directory. `gh copilot suggest "list directories under ~/homework/ with their modification dates"`. Explain. Verify. Run. Handoff condition: produces a list; no operation on files.
- **Step 2:** Dry-run move. Specification names exclusions and output format. Explain. Verify. Run with `-print` or `--dry-run`. Handoff condition: prints expected file list; matches Step 1 survey.
- **Step 3:** Real move. Specification reuses Step 2 with execution. Explain. Verify (compare to Step 2 output). Run. Handoff condition: file count matches; no files left in source dir matching the criteria; no files outside destination dir match the criteria.
- **Step 4:** Post-build verification. Author the post-build learning document.

Within this sequence, the chapter must show:
- One handoff condition failure → revert and respecify.
- One scope-creep moment ("while we're here, want me to also clean up X?") → add to CLI.md, decline.
- One plausibility-auditing moment (something feels off in an explain output) → investigation → either resolved or revert.

---

## 4. The Book's Thesis Connection

Ch 12 is the chapter where the thesis becomes lived experience. The reader has been told what conducting means (Ch 4), named the capacities (Ch 5), built the artifact (Ch 6), formulated (Ch 7), specified (Ch 8), set conditions (Ch 9), and planned (Ch 11). Ch 12 is where they *do it*.

The chapter's contribution:

1. **Executing the discipline.** Every conducting move from the framework appears in execution. The chapter is the framework in motion.
2. **Operational rules for moments of doubt.** Revert-and-respecify, two-failed-corrections, scope-creep-to-CLI.md. These rules are how the discipline survives contact with real builds.
3. **The post-build moment.** The chapter ends with the post-build document (Ch 13's territory in detail). The reader's first complete build is the artifact the rest of the book has been pointing to.

Student-supplied capacity: at every step, the student decides whether to proceed, revert, respecify, or stop. None of these decisions can be transferred to the tool. The chapter is the discipline of *deciding*, made visible.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: W. Edwards Deming (1900–1993).** Strong fit. Keep as primary.

Candidates:

- **W. Edwards Deming** (1900–1993, USA, engineer/statistician). The named figure. PDCA, *Out of the Crisis*, the 14 Points. Deming's career was the discipline of quality built into processes through verification at every step. Famous in operations management, less famous to students. Diversity: white male American. Substantive fit is strong.
- **Taiichi Ohno** (1912–1990, Japan, industrial engineer). Father of the Toyota Production System; originator of the andon cord. The chapter's "stop and revert" discipline is more directly Ohno's than Deming's. Diversity: Japanese, mid-20th-century. **Strongest diverse alternate.** Wikipedia-accessible.
- **Walter Shewhart** (1891–1967, USA, physicist/statistician). Originated the PDCA cycle before Deming popularized it. Statistical process control. More academic than Deming or Ohno. Diversity: white male American. Substantive fit is excellent; cultural reach is narrower.

Recommendation: **consider Taiichi Ohno.** The andon-cord discipline is the chapter's exact operational stance, and Ohno helps the diversity spread (the current TIKTOC list skews white and Anglo-American). Deming is excellent and well-known; Ohno is excellent and under-cited. Author's call.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
The full Act Two framework plus Ch 11's planning. Ch 12 is the synthesis chapter for execution.

### Common misconceptions to disarm
- **"If the plan was good, execution is automatic."** No. Execution is where plans meet reality. The plan was a hypothesis; execution tests it.
- **"Revert is failure."** No. Revert is the discipline working. The build that reverts twice and ships a clean third attempt is a successful build. The build that "corrects forward" through three failures and ships brittle is a failed build with a passing exit code.
- **"Scope creep is fine if the addition is good."** No. The addition might be good; it is not in this build. The chapter's prescription: capture it in CLI.md for later, decline it now.
- **"Plan-mode handles all of this."** No. Plan-mode shows what *the tool* would do. The chapter's discipline is what *the student* must do regardless.

### Effective instructional sequences
- **Seth's full execution transcript.** Annotated. Every command labeled with the capacity being exercised. Every handoff condition labeled with pass/fail/revert. The chapter's spine.
- **The three pivotal moments.** TIKTOC names them: handoff failure → revert. Scope creep → CLI.md, decline. Plausibility-auditing → investigate. The chapter must give each its own labeled segment.
- **Reader runs alongside Seth.** Optional but powerful: the chapter sets up Seth's build such that the reader can run their own version with adaptations and compare.

### Known failure modes
- **The chapter as transcript dump.** Raw transcripts are unreadable. Every command should be embedded in narrative with the capacity-label.
- **Romanticizing the difficulty.** A first build going wrong twice is normal, not heroic. The chapter must keep the emotional register matter-of-fact.
- **Over-promising the rules.** The two-failed-corrections rule is a guideline, not a law. The chapter should frame it as "the typical student who corrects three times is usually working from a bad formulation; restate."

### What separates understanding from memorization
A reader who *understands* Ch 12 can describe, for a build they have not yet attempted, where the scope-creep risk lives and how they will handle it. A reader who memorized Ch 12 can recite the rules without applying them when their own build deviates from plan.

---

## 7. Representation and Display Research

TIKTOC specifies **no figure** for Ch 12. The chapter is execution-narrative-driven; transcripts and annotated commands carry the content.

If the author wants a visual, a useful candidate is a *build-state diagram*: planned → in progress → check → revert / continue / complete. Optional.

---

## 8. Open Questions and Research Gaps

- **Seth's specific build (TIKTOC OQ-1).** Most acute in Ch 12. The chapter is the build, executed.
- **Plan-mode integration.** Author should run Seth's actual build with `copilot` plan mode and document the experience for the chapter. The chapter's treatment of plan-mode depends on this.
- **The "while we're here" rule across editions.** As CLIs become more agentic, the rule's frequency of application increases. Author may want to specify how aggressively to apply the rule (every suggestion? only file-touching suggestions? only destructive?).

---

## 9. Sourcing Notes

- **Deming 1986** *Out of the Crisis* — MIT Press, 2nd ed. (2018) is the most common reprint.
- **Spear & Bowen 1999** — HBR archive; some open versions.
- **Allspaw & Robbins 2010** — O'Reilly.
- **Beyer et al. 2016** *SRE book* — open access at sre.google.
- **Ohno 1988** *Toyota Production System* (the English translation of Ohno's foundational text) — Productivity Press. Cite if swapping to Ohno.
- **Etsy blameless post-mortem**: cite via Etsy engineering blog (codeascraft.com) — stable but check URL.
