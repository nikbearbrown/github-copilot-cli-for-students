# Research: Chapter 5 — The Five Supervisory Capacities
## GitHub Copilot CLI for Students: A Practitioner's Guide

**Chapter one-line:** These are the five things you do that `gh copilot suggest` cannot. Name them. Practice them. Never delegate them.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Engelbart, Douglas C. "Augmenting Human Intellect: A Conceptual Framework." Stanford Research Institute, 1962.** The foundational text for the chapter. Engelbart's H-LAM/T system (Human using Language, Artifacts, Methodology, in which he is Trained) is precisely the framework the five capacities decompose. The supervisory capacities are not new — they are Engelbart's "C-work" (composing, comprehending, computing) made operational for AI-assisted terminal work. Open access: <https://www.dougengelbart.org/content/view/138/>
- **Norman, Donald A. *The Design of Everyday Things*, rev. ed. Basic Books, 2013.** Norman's "gulf of execution" (between intent and action) and "gulf of evaluation" (between action and result) map directly onto Problem Formulation (gulf of execution) and Plausibility Auditing + Interpretive Judgment (gulf of evaluation). The five capacities are the operations that close both gulfs for terminal AI.
- **Klein, Gary. *Sources of Power: How People Make Decisions*. MIT Press, 1998.** Klein's recognition-primed decision (RPD) model: experts decide by recognizing patterns and rapidly simulating outcomes. Plausibility Auditing is RPD applied to AI output. Klein's empirical studies of firefighters, nurses, and military commanders are the model for what "hearing the wrong note" actually is cognitively.
- **Anthropic RCT (Ch 1).** The high-scoring patterns are operations the chapter names by other words: follow-up question = Plausibility Auditing surfacing as a question; code+explanation = Interpretive Judgment in action; conceptual-only AI = Tool Orchestration with the right scope.
- **Hutchins, Edwin. *Cognition in the Wild*. MIT Press, 1995.** "Distributed cognition." The student-plus-CLI is a distributed cognitive system. The five capacities are the cognitive operations that *cannot* be distributed to the CLI part of the system. Helpful for the chapter's framing without being explicitly cited.

### Key empirical cases

- **The five capacities applied to the Knight Capital incident (cited Ch 4).** Plausibility Auditing failure: nobody felt the wrong note when a flag was reused. Tool Orchestration failure: the deployment system ran without per-server verification. Executive Integration failure: nobody held the whole system in their head. Useful for showing the capacities aren't abstract — they're what was missing in real disasters.
- **The five capacities applied to the GitLab 2017 incident (Ch 1).** Specifically: Plausibility Auditing should have caught the "this seems too easy" feeling before the `rm -rf` on production. Interpretive Judgment should have caught that the hostname matched production. The case is a five-capacity post-mortem in everything but name.
- **Seth's worked example (Ch 5 opening per TIKTOC).** Seth almost deploys a `gh copilot suggest` output. Something feels off. Plausibility Auditing is the capacity that catches it. Author should develop one specific concrete instance (the exact command, the exact gut feeling, the exact thing that was wrong).

---

## 2. The Core Concept — State of the Field

### What is settled

- **Human-machine task division has been studied for 60+ years.** From Fitts (1951) "MABA-MABA" (Men Are Better At — Machines Are Better At) lists to current human-factors taxonomies. The five-capacity framework is in the lineage but specifically operationalized for LLM-assisted terminal work.
- **Pattern recognition and judgment are different cognitive operations.** Established in cognitive science (Klein, Kahneman). The five capacities are five flavors of judgment that don't reduce to pattern recognition.
- **Naming a capacity helps deploy it.** Vocabulary studies in expert pedagogy (Ericsson, Charness) show that named distinctions are more reliably noticed and applied. The chapter's main pedagogical bet is that naming the five capacities makes them more deployable than referring to "judgment" generically.

### What is disputed

- **Whether five is the right number.** Some frameworks use three (judgment / verification / integration). Some use seven or more. The book's argument for five is operational, not theoretical: the five named capacities are the ones that arise distinctly in terminal-AI workflows. Acknowledge in a footnote that the framework is a synthesis, not a finding.
- **Whether the capacities are stable as tools improve.** TIKTOC acknowledges this — Contested Claims table: "Five supervisory capacities are permanent — Emerging — 'Currently requires human judgment'." Good framing. Hold the position lightly.
- **Whether the capacities are distinguishable in practice.** Some readers will struggle to distinguish Plausibility Auditing from Interpretive Judgment in real cases. The chapter's exercise design must reckon with this.

### What has changed recently (last 5 years)

- The Anthropic 2026 RCT provides the first empirical separation of high- and low-scoring AI-interaction patterns. The five capacities are a finer-grained version of what Anthropic measured at three high-scoring patterns. The book is operationalizing the framework Anthropic empirically validated.
- Agentic CLI surfaces (post-January-2026) change the *opportunities* for exercising capacities (more chained operations, more decisions per minute) without changing the capacities themselves. The chapter should note this explicitly.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 5 sits in Git automation per TIKTOC.)

For each capacity, one Git-domain example:

- **[PA] Plausibility Auditing — the `git push --force` that feels wrong.** The CLI generates `git push --force origin main` to "clean up the commit history." Explanation is correct. Something feels off. PA: this rewrites history on a shared branch. Investigate before running.
- **[PF] Problem Formulation — what does "clean slate" actually mean?** Before any `gh copilot suggest` invocation: am I trying to undo the last commit, discard local changes, or reset to a specific tag? The same English phrase maps to four different Git commands. PF decides which.
- **[TO] Tool Orchestration — when to use ask vs. suggest vs. write it yourself.** For "show me what files changed since yesterday," `gh copilot ask` is the right tool (read-only interrogation). For "generate the diff command," `gh copilot suggest`. For a one-line script the student knows by heart, no AI invocation at all. TO chooses.
- **[IJ] Interpretive Judgment — the CLI's explanation is generic; my repo isn't.** Explain says "this command rebases the current branch onto main." The student knows their repo has a submodule that rebases don't handle cleanly. IJ supplies the meaning the explanation doesn't.
- **[EI] Executive Integration — the build still touches main.** Three prompts ago, the student established this script should never touch main. The new suggestion operates on main. EI holds the goal across prompts and stops the build.

---

## 4. The Book's Thesis Connection

Ch 5 is the chapter that makes the supervisory work *namable*. The thesis claim ("the student who maintains a CLI.md and applies the gate builds capability; the student who runs without explain atrophies") depends on the supervisory capacities being identifiable. If the reader cannot name what they are doing when they conduct, they cannot tell whether they are doing it.

The chapter's contribution to the thesis:

1. **Decomposition.** "Judgment" is too vague to practice. The five capacities are five practicable operations.
2. **Diagnostic vocabulary.** When a build goes wrong, the reader can ask: which capacity was absent? The capacities turn post-mortems from "I should have been more careful" into "I should have exercised PA at step 3."
3. **Stability.** The CLI changes; the capacities are tool-agnostic. The chapter's framework outlasts the syntax of the moment.

Student-supplied capacity (now explicit): all five capacities require knowledge of the student's actual environment, goals, prior decisions, and tolerance for loss. None of these are in the prompt. None can be transferred.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Douglas Engelbart (1925–2013).** Perfect fit. Keep as primary.

Candidates:

- **Douglas Engelbart** (1925–2013, USA, computer engineer). The named figure. *Augmenting Human Intellect* (1962) is the conceptual ancestor of the entire framework. Famous-tier to computer scientists, lesser-known to high schoolers, especially in this specific framing (his fame is more for the mouse and the Mother of All Demos). Diversity: white male American. Strongest substantive fit in the entire 15-chapter set.
- **Lucy Suchman** (born 1951, USA, anthropologist of work and technology). *Plans and Situated Actions* (1987). Suchman's argument that human work is *situated* — context-dependent in ways plans cannot capture — is the framework for why the five capacities cannot be transferred to the CLI. Diversity: woman, American, ongoing scholar. Lesser-known than Engelbart. Strong substantive fit. **Strongest diverse alternate.**
- **Vannevar Bush** (1890–1974, USA, engineer). "As We May Think" (1945). Bush imagined the Memex — a tool that augments memory and association. The chapter is about preventing augmentation from becoming substitution. Diversity: white male American, mid-20th-century.

Recommendation: **keep Engelbart.** Substantive fit is overwhelming. If the diversity audit across the full set demands a swap here, Suchman is the strongest substitute. Bush is too aspirational and not specifically about supervisory work.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
Ch 4's gate. The reader should already be running suggest→explain→verify in some form. Ch 5 names what they were already doing in the explain step.

### Common misconceptions to disarm
- **"These are five things to check on every command."** No — they are five operations that arise at different moments. PF dominates at problem formulation; PA fires when something feels off; TO fires at tool selection; IJ fires when interpreting explain; EI runs throughout. The chapter must show this.
- **"I have to consciously perform all five every time."** Eventually they fuse into a single conducting practice. Early on, naming each helps; later, the names recede.
- **"The five capacities are like personality traits — some people have them, some don't."** They are practices. They develop with use. The chapter must frame them as exercise, not as inventory.

### Effective instructional sequences
- **Define each capacity by example, not abstraction.** TIKTOC's content for each capacity is already structured this way — short definition, then a specific terminal moment. Follow this.
- **Trace a build through all five.** The chapter's exercises should walk a student through one transcript and have them label each step with the capacity being exercised. This is the chapter's central Apply-level exercise.
- **Diagnostic exercise.** Given a build that went wrong, have the student name which capacity was absent. Inverts the trace — same skill, different angle.

### Known failure modes
- **The chapter as a glossary.** If the five capacities read as a list of definitions, the reader memorizes the names and forgets the operations. The chapter must lead with action.
- **Over-specification.** The capacities are usefully fuzzy. The chapter should not give precise decision rules ("if X happens, exercise PA; if Y, exercise IJ"). Decision rules are brittle; capacities are durable.
- **Conflating PA with paranoia.** PA is hearing the wrong note. It is not "check everything always." The chapter must distinguish vigilance from anxiety.

### What separates understanding from memorization
A reader who *understands* Ch 5 can read a transcript they haven't seen before and label which capacity is being exercised at each step *and* identify which capacity was absent if the build went wrong. A reader who memorized Ch 5 can recite the five names and definitions but can't apply them diagnostically.

---

## 7. Representation and Display Research

TIKTOC specifies one figure:

- **`<!-- → [DIAGRAM: Five supervisory capacities as a five-column layout.] -->`** Worked content:

  Five vertical columns, each containing:
  | [Abbrev] | Plain Name | Terminal-specific one-sentence definition |
  |---|---|---|
  | PA | Plausibility Auditing | Hearing the wrong note in a generated command before running it. |
  | PF | Problem Formulation | Deciding what the task IS before `suggest` ever sees it. |
  | TO | Tool Orchestration | Choosing which CLI command, with what context, in what order. |
  | IJ | Interpretive Judgment | Supplying the meaning the CLI's explanation cannot. |
  | EI | Executive Integration | Holding the whole build toward a single goal across many prompts. |

  Editorial style. Five equal columns. No color. The abbreviations should be visually consistent with how they appear in the body text.

No table required beyond this.

---

## 8. Open Questions and Research Gaps

- **Empirical separability.** No published study directly measures whether the five capacities are reliably distinguishable. The framework is a synthesis from Engelbart, Norman, Klein, and the Anthropic RCT's three engagement patterns. Acknowledge the synthesis explicitly.
- **Tool-version dependence.** As of the new `copilot` CLI (Jan 2026), some capacity exercise happens at the *plan-approval* moment rather than at the per-command moment. The capacities don't change; the surfaces where they fire shift. The chapter should note this.
- **Capacity-by-context mapping.** The chapter would benefit from a 5x5 grid mapping each capacity to a moment in a typical build. The author may want to commission this for a sidebar.

---

## 9. Sourcing Notes

- **Engelbart 1962** report is open access via the Doug Engelbart Institute. Stable URL.
- **Norman 2013** rev. ed. is the standard citation; original 1988 *Psychology of Everyday Things* is also used in some pedagogy literature — be consistent.
- **Klein 1998** is the trade book; the academic foundation is in Klein's papers on RPD model (1989, 1993). Cite both if depth allows.
- **Hutchins 1995** is canonical in distributed cognition; the *Ship's Cognition* example is the relevant illustration.
- **Anthropic RCT** already cited Ch 1.
