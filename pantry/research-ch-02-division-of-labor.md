# Research: Chapter 2 — What You're Actually Good At (And What `gh copilot suggest` Is Better At)
## GitHub Copilot CLI for Students: A Practitioner's Guide

**Chapter one-line:** Pattern completion is the CLI's domain. Scope judgment is yours. Knowing which is which is the whole game.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Polanyi, Michael. *The Tacit Dimension*. University of Chicago Press, 1966.** Polanyi's claim — "we can know more than we can tell" — names exactly what the CLI cannot do. The student knows their filesystem and their intent tacitly; the CLI sees only the prompt. The chapter's labor split is Polanyi's split between articulable knowledge (which the model has) and tacit knowledge (which the student has).
- **Brynjolfsson, Erik & Andrew McAfee. *The Second Machine Age*, ch. 12 ("Learning to Race with Machines"). Norton, 2014.** The complement-vs-substitute frame. Useful for one paragraph; the chapter shouldn't read like a Brynjolfsson summary.
- **Dreyfus, Hubert L. *What Computers Still Can't Do: A Critique of Artificial Reason*. MIT Press, 1992.** Dreyfus's central argument — that human expertise is contextual and embodied, not rule-following — is the philosophical foundation for the supervisory capacities. The book is dated in its specific AI predictions but the *structural* claim still holds for LLMs: pattern completion ≠ situated judgment.
- **Bommasani, Rishi et al. "On the Opportunities and Risks of Foundation Models." Stanford CRFM, 2021 (arXiv:2108.07258), §1.1.** The foundation-models report's framing of LLMs as systems that learn correlations across very large corpora. Useful for the technical sentence "the CLI is superhuman at pattern completion because it has seen more code than any human ever will" without overclaiming about understanding.
- **GitHub. "How AI is improving the developer experience." Developer skills survey, 2023–2024.** GitHub's own reporting on developer behavior with Copilot. Useful for the empirical sentence "the speed gap between AI generation and human verification is widening." Cite carefully — vendor research, treat as evidence about adoption, not about quality.

### Key empirical cases

- **The `rm -rf $VAR/` Steam Linux bug (2015).** The Steam Linux client contained a bash script with `rm -rf "$STEAMROOT/"*`. If `$STEAMROOT` was unset (which happened in some configurations), the script ran `rm -rf /*`. The script was syntactically correct, the *pattern* was right, the scope was catastrophically wrong. Public bug report: github.com/ValveSoftware/steam-for-linux/issues/3671. Anchor case for "pattern complete, scope wrong."
- **The `find -exec` permission-change incident.** Common student-level failure: running `find . -name '*.py' -exec chmod 755 {} \;` from one directory above the intended one. Pattern is correct. Scope is wider than intended. Generalizable; the author can synthesize a specific worked example.
- **Git automation cases (mapped to TIKTOC domain coverage: Ch 2 sits in Git automation).** A `git filter-branch` or `git filter-repo` command suggested by the CLI runs cleanly but rewrites more commits than the student intended. The rewritten history is now in their local repo; if pushed, it propagates. Documented broadly in Git mailing list archives.

---

## 2. The Core Concept — State of the Field

### What is settled

- **LLMs excel at pattern completion.** The empirical literature on code generation (HumanEval, MBPP, the Code Llama and StarCoder evaluations) consistently shows that for well-specified small tasks, LLM code completion approaches or matches mid-level human performance.
- **LLMs do not have a stable model of the user's environment.** They cannot see the filesystem, the working directory state, the git branch, or the user's prior intent. Anything they "know" must be in the prompt.
- **Scope judgment requires knowledge the model doesn't have.** This is settled by structure, not by experiment: the user's filesystem and intent are not in the prompt unless the user puts them there.

### What is disputed

- **How well agentic tools partially close the gap.** Newer agentic surfaces (including the post-January-2026 `copilot` CLI in plan/autopilot mode) read files, list directories, and acquire some environment context before acting. They close *part* of the gap but not the intent gap — the model still does not know which files matter to the user. The chapter should not treat agentic mode as solving the supervisory problem.
- **Whether "structural blindness" is permanent.** Some claim multi-modal agents will eventually have full filesystem access and remove the blindness. The book stakes a middle position: even with full access, the model does not have the user's *intent* — which is unknowable to anyone but the user — so supervisory capacity remains.

### What has changed recently (last 5 years)

- The 2026 generation of CLI tools can read files, list directories, and chain operations more autonomously. This raises stakes for scope judgment, not lowers them — the same `find` that used to be one line is now a multi-step plan executed without per-step approval unless the user intervenes.
- Public discussion of the labor split has matured. The 2024–2026 wave of developer-skill research (Anthropic RCT especially) gives the book empirical ground that didn't exist when *The Second Machine Age* was written.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 2 sits in Git automation.)

- **`git filter-repo` over too many commits.** Student wants to remove a secret from the last commit. CLI suggests `git filter-repo` operating on the whole history. Exit 0. The history is now rewritten. Pushed: the team's history is rewritten too.
- **`git reset --hard` to a stale branch.** Student asks for "a clean slate, undo my recent changes." CLI generates `git reset --hard HEAD`. Pattern is correct. Student's *intent* was "undo the last commit and keep my staged changes" — that's `git reset --soft HEAD~1` instead. Pattern completion is fine; scope judgment requires knowing what the student means by "clean slate."
- **`git rebase -i` on a shared branch.** CLI generates a rebase command that rewrites commits on `main`. The student's local pre-push state is fine. Pushing the rewritten branch breaks every teammate's local clone. The CLI cannot know the branch is shared.

---

## 4. The Book's Thesis Connection

Ch 2 makes the thesis operational. If Ch 1's claim is "exit 0 ≠ correct," Ch 2's claim is "*here is why* — pattern completion and scope judgment are different cognitive operations, and only one of them is transferable to the model."

What only the student can supply:
- Which files in their actual repo are theirs to modify and which belong to teammates.
- What "old logs" means in their actual project (5 days? 30? Last semester?).
- Whether the consequence of a wider-than-intended scope is acceptable (school assignment vs. shared production branch).
- What the *task* is — distinct from what the *prompt* asks for. (The classic mismatch: "I asked for X; what I needed was Y.")

The five supervisory capacities (PA / PF / TO / IJ / EI) get named in Ch 5, but Ch 2 must already be operating with them in plain English. The chapter introduces *scope judgment* as a single concept and lets Ch 5 decompose it.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Frederick Winslow Taylor (1856–1915).** Acceptable but worth challenging.

Candidates:

- **Frederick Winslow Taylor** (1856–1915, USA, mechanical engineer). The named figure. *Principles of Scientific Management* (1911). Taylor's project was exactly the labor split — which work belongs to the engineer and which to the worker / machine. **Weakness:** Taylor's legacy is contested (dehumanization of labor, time-and-motion studies). For a high school student reader, the association may carry baggage the chapter does not want. The intellectual fit is strong; the cultural valence is awkward.
- **Lillian Moller Gilreth** (1878–1972, USA, industrial/organizational psychologist). Co-developed time-and-motion methods with husband Frank Gilbreth, then pioneered ergonomics — the *humane* version of Taylor's project. She studied which tasks suited the human and which suited the machine. Diversity: woman, American, early-20th-century. Lesser-known than Taylor. Wikipedia-accessible. **Strongest alternate candidate** for this chapter.
- **Hubert Dreyfus** (1929–2017, USA, philosopher). *What Computers Still Can't Do* is the most direct intellectual ancestor of the chapter's argument. Diversity: white male American philosopher, mid-20th-to-21st-century. Famous to philosophers, lesser-known to high school students. Strong intellectual fit; weak diversity contribution.

Recommendation: **swap Taylor for Lillian Gilreth.** Gilreth's work is the empirical foundation for "which tasks suit the human, which suit the tool" *and* she helps the diversity spread of the full 15-figure set (TIKTOC's existing list skews white and male).

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
Some Git fluency (add, commit, push, branch) — assumed by TIKTOC. The chapter should not teach Git; it should use Git as the domain in which to teach scope judgment.

### Common misconceptions to disarm
- **"Pattern completion is just a fancy name for what the model does."** It's specific: pattern completion is producing the next plausible token given context. Scope judgment is deciding whether the resulting command should run *in this specific situation*. Different operations.
- **"If I give the CLI enough context, it can do scope judgment too."** Even with full context, the CLI cannot know what the student *means*. Intent is held only by the user. Context narrows hallucination; it does not transfer intent.
- **"Pair-programming with the CLI is the answer."** Pair programming with a human partner exchanges intent through conversation. Pair-with-CLI exchanges prompts and outputs. Different.

### Effective instructional sequences
- **Two-column classification exercise.** Ten terminal tasks; reader classifies each as CLI-domain, human-domain, or dangerous-middle. This is the chapter's Apply-level exercise and probably its best learning vehicle.
- **Same task, two outcomes.** TIKTOC's worked example pattern (same `find` command, one student conducts, one doesn't, different outcomes) is exactly right. The chapter should land it before the framework is named.

### Known failure modes
- **The chapter as a list of capacities.** Ch 5 names the five capacities. If Ch 2 lists them already, Ch 5 is redundant. Ch 2 should operate in plain English ("scope judgment") and earn the named decomposition later.
- **Sounding like a manifesto.** "What you're actually good at" is intentionally cheeky; the chapter must not become a self-help essay about the value of being human. It's a technical chapter about a specific cognitive distinction.

### What separates understanding from memorization
A reader who *understands* Ch 2 can take a new terminal task they encounter outside the book and decompose it into pattern-completion work and scope-judgment work without prompting. A reader who memorized Ch 2 can repeat the labor-split lists but can't apply the distinction to a task they haven't seen.

---

## 7. Representation and Display Research

TIKTOC specifies one figure:

- **`<!-- → [TABLE: Division of labor — two columns: CLI does / Human does.] -->`** Worked content:

  | CLI does | Human does |
  |---|---|
  | Pattern completion (next-token, common idioms) | Scope definition (which files, which directories) |
  | Syntax generation (correct flag order) | Target specification (this repo, this branch, this filesystem) |
  | Flag lookup (`-print`, `-mtime`, `-exec`) | Exclusion decisions (what must not be touched) |
  | Command structure (pipe chains, subshells) | Intent verification (does this match what I meant?) |
  | Idiomatic translation (English → command) | Consequence assessment (is the cost of being wrong acceptable?) |

The table is the chapter's spine. No additional displays required for content.

---

## 8. Open Questions and Research Gaps

- **Agentic-mode framing.** The post-January-2026 `copilot` CLI in plan/autopilot modes acquires more environment context than the retired `gh copilot suggest`. The chapter must address this without conceding the supervisory-judgment claim. Recommendation: state that more context narrows hallucination but does not transfer intent, and that the dangerous-middle pattern grows *more* important with agentic surfaces, not less.
- **Empirical specificity for the labor split.** No published study cleanly measures the pattern/scope distinction the way the chapter describes it. The split is a defensible synthesis of the foundation models / cognitive science literature, but it is not directly measured. Acknowledge in a footnote.
- **The Lillian Gilreth swap.** Recommended above. The author should decide before drafting whether to keep Taylor (intellectually clean, culturally fraught) or swap to Gilreth (more diverse, equally well-fit, less well-known to students). My recommendation is to swap; the author may have reasons to keep Taylor.

---

## 9. Sourcing Notes

- **Polanyi 1966** is canonical philosophy of science; widely cited. Use sparingly — one sentence at most.
- **Dreyfus 1992**: confirm 2nd edition. The 1972 first edition (different title: *What Computers Can't Do*) was famously wrong about chess; the 1992 second edition added a preface that anticipates much of the LLM era. The 1992 edition is the citation the chapter wants.
- **Bommasani 2021 foundation models report**: very long, frequently re-versioned on arXiv. Cite a specific arXiv version (v3 is standard).
- **Steam `rm -rf` bug**: linked above. Public, stable.
- **GitHub developer survey 2023–2024**: vendor research. Useful for adoption stats, not for quality claims.
