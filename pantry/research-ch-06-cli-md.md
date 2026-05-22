# Research: Chapter 6 — CLI.md: Your Terminal Constitution
## GitHub Copilot CLI for Students: A Practitioner's Guide

**Chapter one-line:** CLI.md is the file you maintain and paste from. It is the difference between a `gh copilot suggest` session that knows your project and one that guesses.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Knuth, Donald E. "Literate Programming." *The Computer Journal* 27, no. 2 (1984): 97–111.** The founding paper. Knuth's argument: software should be written for humans first, with code as a side effect. CLI.md inverts the relationship — it is the human-facing context that makes the AI's code generation match the project — but the spirit is the same. The chapter's intellectual lineage runs through this paper.
- **Anthropic. CLAUDE.md documentation (claude.com/docs).** The closest comparable: a file Claude Code reads automatically as project context. Useful for the chapter's contrast — what changes when injection is automatic vs. manual. The book's argument is that *manual* paste is pedagogically more honest because it forces Tool Orchestration exercise on every invocation.
- **OpenAI. AGENTS.md / Codex documentation.** The Codex equivalent. Same comparison: automatic context injection. The contrast lets the chapter make a specific claim about CLI.md's pedagogical advantage.
- **Brooks, Frederick P. *The Mythical Man-Month*, 20th anniversary ed. Addison-Wesley, 1995, esp. "The Documentary Hypothesis."** Brooks's argument that documents (specs, schedules, budgets) are not byproducts of work but the *substrate* on which work happens. CLI.md is a Brooksian document for terminal work — the place where decisions are made and recorded.
- **Hutchins, Edwin. *Cognition in the Wild*, ch. 5 ("The Organization of Team Performances"). MIT Press, 1995.** The cognitive-anthropology argument that external representations (charts, logs, checklists) extend the cognition of the team. CLI.md is the external representation that makes the student-plus-CLI system cognitively coherent across sessions.

### Key empirical cases

- **The Atul Gawande / WHO Surgical Safety Checklist (2009).** Documented in *The Checklist Manifesto*. Two-minute checklist; surgical complication rates dropped one-third in eight pilot hospitals. The chapter's intellectual cousin: a small, written, persistent artifact that improves outcomes by forcing the practitioner to engage with context. The CLI.md is the terminal version. Strong precedent for "external memory is not extra work."
- **The "runbook" tradition in operations engineering.** Standard practice at Google, Netflix, Amazon: every on-call rotation has runbooks. The runbook is what makes one person's hard-won knowledge usable by the next person on shift. Open-access references include Google's *Site Reliability Engineering* (Beyer et al., 2016) — especially Part III on practices.
- **Seth's first CLI.md (per TIKTOC's worked example).** Four sections; populated for a real student file-automation project; demonstrably better second session. Author should ensure Seth's actual first CLI.md is preserved for inclusion as a worked artifact, not regenerated for the book.

---

## 2. The Core Concept — State of the Field

### What is settled

- **External memory aids reduce error rates and increase consistency.** Established in human-factors and pilot-error literature (Reason 1990, Norman 1988, Gawande 2009). The book's claim that CLI.md helps is in the empirical lineage of these findings.
- **Project-specific context improves LLM output.** Established across the prompt-engineering literature (Anthropic, OpenAI, GitHub published guidance 2023–2025). The novel claim is *how* the context is maintained, not whether it helps.
- **Manual maintenance forces engagement.** The Anthropic RCT's high-scoring patterns (follow-up questions, code+explanation) all involve manual user engagement. The book's argument that *manual* paste is a feature, not a limitation, is consistent with this finding.

### What is disputed

- **Manual vs. automatic context injection.** Vendors generally prefer automatic (CLAUDE.md, AGENTS.md, IDE-integrated context). The book takes a contrarian position: for *pedagogical* purposes, manual is better because it forces TO exercise. This is a defensible position but should be argued explicitly, not asserted.
- **The right size of project-context documents.** Some teams use 500-line context files; others use 50. No empirical optimum. The book recommends compactness (the four-section format), which is consistent with checklist-manifesto practice.
- **Whether CLI.md should be checked into version control.** Open question. Pro: the project's terminal conventions become version-controlled. Con: lessons-learned often include sensitive details the team may not want versioned. Recommendation: yes, with a `[private]` section the student can git-ignore.

### What has changed recently (last 5 years)

- The widespread adoption of CLAUDE.md / AGENTS.md / similar auto-injection mechanisms (2023–2026) is the immediate context the book responds to. The book's CLI.md is not behind the curve — it is a deliberate choice to operate at a different level of explicitness.
- The retirement of `gh copilot suggest/explain/ask` in Jan 2026 does *not* affect CLI.md. The new interactive `copilot` CLI does not read CLI.md automatically; the file remains a paste source. The chapter's argument survives the tool transition cleanly. This is one of the chapter's strongest points — note it explicitly.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 6 isn't tied to one domain; CLI.md is a structural artifact applied across all of Seth's chapters.)

- **Seth's file-automation project (TIKTOC's worked example).** First session: `gh copilot suggest` produces commands that don't match his directory structure. Seth writes a CLI.md with environment facts and exclusions. Second session: pasted-context produces commands that match the structure immediately.
- **Git automation CLI.md section.** Project conventions: "main is protected; never push --force to main; always rebase feature branches before merge." A student CLI.md with these rules turns into a paste-ready paragraph for every Git-related suggest invocation.
- **Cross-platform CLI.md.** Student works on macOS at home, Linux at school. CLI.md has an Environment section listing both. Suggest prompts paste the relevant environment depending on which machine the student is on. The CLI does not need to infer; the student tells.

---

## 4. The Book's Thesis Connection

Ch 6 is where the conducting discipline becomes *durable*. The gate (Ch 4) is per-command; the capacities (Ch 5) are per-decision; CLI.md is the artifact that persists across sessions. Without persistence, the student rebuilds context every time and the system is fragile. With CLI.md, the discipline accumulates.

The chapter's contribution to the thesis:

1. **The artifact is the discipline made visible.** The student who maintains CLI.md is performing supervisory work. The student who doesn't may still be conducting in their head, but there is no external record. Brooks's argument applies: the document is the work.
2. **Manual paste exercises Tool Orchestration on every invocation.** This is the chapter's strongest claim. Without automatic injection, the student must decide each time what context to provide. The pedagogical asymmetry with CLAUDE.md / AGENTS.md is the chapter's defining argument.
3. **Lessons-learned accumulation is irreversible capability growth.** A CLI.md that records what went wrong yesterday is the mechanism by which a single mistake becomes durable knowledge. Without CLI.md, the mistake happens again. The chapter's emotional payoff is in this section.

Student-supplied capacity: the student is the only person who knows what their project's conventions, exclusions, and prior mistakes are. CLI.md is the form in which that knowledge survives.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Donald Knuth (born 1938).** Strong fit. Keep as primary.

Candidates:

- **Donald Knuth** (born 1938, USA, computer scientist). The named figure. *The Art of Computer Programming*. Literate programming. Knuth's argument that human-readable explanation belongs alongside (or above) code is the chapter's intellectual core. Famous-tier; this slightly disfavors per criteria but the substantive fit overrides. Diversity: white male American.
- **Margaret Hamilton** (born 1936, USA, software engineer). Apollo flight software. Coined "software engineering." Hamilton's insistence on rigorous, documented, fault-tolerant code is the chapter's discipline applied at NASA scale. Diversity: woman, American, mid-20th-century. **Strongest diverse alternate.** Wikipedia-accessible.
- **Ward Cunningham** (born 1949, USA, programmer). Invented the wiki. *Cunningham's Law*: the easiest way to get the right answer is to post the wrong one. CLI.md is a wiki-in-miniature for one student's project. Famous-to-developers but not to high schoolers. Diversity: white male American.

Recommendation: **consider swapping to Margaret Hamilton.** Hamilton's documentation-as-discipline at Apollo is at least as strong a fit as Knuth's literate programming, and Hamilton helps the diversity spread. Knuth is excellent; Hamilton is excellent and more under-cited.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
The student must already see the gate (Ch 4) and the capacities (Ch 5) as worth doing. Ch 6 is where they accumulate the artifact that makes both repeatable.

### Common misconceptions to disarm
- **"CLI.md is documentation for the CLI to read."** No. CLI.md is for the *student* to read and paste from. The CLI does not read it automatically. This is the chapter's most important pedagogical point.
- **"More content is better."** No. The four-section format is the recommendation. A 50-line CLI.md the student actually maintains beats a 500-line CLI.md they wrote once and abandoned.
- **"CLI.md is for big projects."** No. A student CLI.md for a homework directory is appropriate scope. The discipline scales down.
- **"I'll start CLI.md when the project is bigger."** The chapter must counter this directly. Start CLI.md at minute one of the project, even if it's three lines.

### Effective instructional sequences
- **Write CLI.md, then use CLI.md, in one session.** The chapter's central exercise should have the reader write a four-section CLI.md for a current project, then run a suggest with it and a suggest without it, and observe the difference. Concrete, fast, demonstrates the value.
- **Lessons-learned by example.** Show three lessons-learned entries from Seth's CLI.md. Each is one date, one mistake, one fix. The format is the lesson.
- **Update CLI.md after every session.** Frame it as habit, not chore. Two minutes after a session, three sentences in CLI.md.

### Known failure modes
- **CLI.md as homework.** If readers experience CLI.md as a writing assignment, they abandon it. The chapter must frame it as the artifact that makes their *next session faster*. Self-interest, not duty.
- **The over-documented CLI.md.** A reader who writes ten pages of context will not maintain it. Recommend brevity in the chapter; show short examples.
- **The never-updated CLI.md.** Without the lessons-learned habit, CLI.md becomes a project-start artifact that ossifies. The chapter must address update cadence — recommend a one-minute review at the end of every session.

### What separates understanding from memorization
A reader who *understands* Ch 6 can write a CLI.md for a project they invented and explain *why* each section is there. A reader who memorized Ch 6 can recite the four sections without producing one tailored to their actual situation.

---

## 7. Representation and Display Research

TIKTOC specifies two figures:

- **`<!-- → [DIAGRAM: CLI.md in the workflow.] -->`** Worked content:

  Horizontal flow: `Project start → write initial CLI.md → first suggest session (paste context) → suggest output matches → session end → update CLI.md with lessons → next session → paste updated context → suggestions improve → loop.`
  
  Below the flow, two parallel rows: "without CLI.md" (CLI guesses; commands don't match; student corrects manually) vs. "with CLI.md" (CLI knows context; commands match; student verifies and runs).

- **`<!-- → [TABLE: CLI.md include/exclude — two columns.] -->`** Worked content:

  | Include | Exclude |
  |---|---|
  | Project overview (one paragraph) | General shell knowledge the CLI already has |
  | Environment facts (OS, shell, directory layout) | Constantly changing state (current branch, current directory) |
  | Command conventions (always X, never Y) | Personal notes unrelated to terminal work |
  | Known dangerous patterns | Comprehensive command tutorials |
  | Lessons learned (date, mistake, fix) | Secrets, API keys, credentials |

Both displays are central to the chapter. No additional displays required.

---

## 8. Open Questions and Research Gaps

- **CLI.md file extension and location.** TIKTOC's OQ-2 calls this out. Recommendation: `.md` extension, project root, optionally `.cli/CLI.md` for projects that want to namespace it. Address in the chapter directly.
- **Sharing CLI.md across team or class.** Useful or distracting? Open question. The book's primary reader is self-directed, so the chapter can focus on personal CLI.md. Sharing is a future-edition topic.
- **CLI.md vs. AGENTS.md vs. CLAUDE.md harmonization.** As of 2026, students may use multiple AI CLIs. Should they maintain one file or three? Recommendation: one CLI.md (or one project-context file under any name) with tool-specific sections. Address in a sidebar.
- **The "private" section question.** Some lessons learned include sensitive details (server names, credential rotation history). Recommend a clearly-marked private section that is git-ignored. Address explicitly.

---

## 9. Sourcing Notes

- **Knuth 1984 paper** is open access via *The Computer Journal* archive.
- **Brooks 1995** anniversary ed. is the standard; the original 1975 essays are also valid citations.
- **Gawande 2009** *The Checklist Manifesto* — trade book, widely available, anchored in the WHO checklist study (Haynes et al., NEJM 2009).
- **Anthropic CLAUDE.md / OpenAI AGENTS.md** documentation: cite the version current at press. These docs evolve.
- **Beyer et al. 2016** *Site Reliability Engineering* — open access at sre.google. Use selectively; the book is dense.
