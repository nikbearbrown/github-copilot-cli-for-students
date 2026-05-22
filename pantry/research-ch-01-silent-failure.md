# Research: Chapter 1 — The Silent Failure: What's Actually Happening
## GitHub Copilot CLI for Students: A Practitioner's Guide

**Chapter one-line:** The most dangerous terminal failure is the one that doesn't look like a failure at all.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Bastani, Hamsa; Bastani, Osbert; Sungu, Alp; Ge, Haosen; Kabakcı, Özge; Mariman, Rei. "Generative AI without guardrails can harm learning: Evidence from high school mathematics." *PNAS* 122 (2025).** Roughly 1,000 Turkish high school students; three arms (control, GPT Base, GPT Tutor with guardrails). GPT Tutor group performed 127% better in *AI-assisted practice* but scored *the same as control* on an unaided exam. **GPT Base performed worse than control on the unaided exam.** This is the empirical anchor for Ch 1's "borrowed performance vs. built capability" claim. The terminal-specific version: a student whose `gh copilot suggest` output works during the assignment is not demonstrating the capability the assignment was supposed to build. Open access: <https://www.pnas.org/doi/10.1073/pnas.2422633122>.
- **Kosmyna, Nataliya et al. "Your Brain on ChatGPT: Accumulation of Cognitive Debt when Using an AI Assistant for Essay Writing Task." arXiv:2506.08872, MIT Media Lab, June 2025.** 54 participants, three groups (LLM / search engine / brain-only), EEG measurement. LLM users showed the weakest brain network connectivity. When LLM users were reassigned to brain-only for a fourth session, they remembered little of the essays they had earlier "written" with ChatGPT. The terminal interpretation: a student who delegates command construction not only doesn't build the mental model — they don't *remember the commands they ran* well enough to operate the terminal unassisted later. Cite as preprint; check for peer review at press.
- **"How AI assistance impacts the formation of coding skills." Anthropic, 2026 (arXiv:2601.20245).** RCT, 52 mostly junior engineers learning Python's Trio asynchronous library. AI-assisted group averaged 50% on a 14-question comprehension quiz; hand-coding group averaged 67%. **Three low-scoring interaction patterns (below 40%):** complete AI delegation, progressive AI reliance, iterative AI debugging. **Three high-scoring patterns (65%+):** follow-up questions after generation, code-with-explanation requests, AI for conceptual questions only. The Anthropic RCT is the most direct empirical support for the suggest→explain→verify gate: the high-scoring patterns *are* the gate. <https://www.anthropic.com/research/AI-assistance-coding-skills>.
- **Risko, Evan F. and Sam J. Gilbert. "Cognitive Offloading." *Trends in Cognitive Sciences* 20, no. 9 (2016): 676–688.** The cognitive-science vocabulary the three empirical studies confirm. Useful for one technical sentence; the studies above carry the argument.
- **Sweller, John. "Cognitive Load During Problem Solving." *Cognitive Science* 12 (1988): 257–285.** Founding paper on cognitive load theory. Explains *why* the silent failure mechanism works: when the tool removes the load, the schema doesn't form.

### Key empirical cases

- **GitLab database deletion (Jan 31, 2017).** Engineer YP-1 ran `rm -rf` against what he thought was a secondary replica. It was production. Five backup mechanisms had silently failed for weeks. Public post-mortem at GitLab: <https://about.gitlab.com/blog/2017/02/01/gitlab-dot-com-database-incident/> and <https://about.gitlab.com/blog/2017/02/10/postmortem-of-database-outage-of-january-31/>. The killer detail for Ch 1: every backup ran nightly, exited 0, and produced empty archives. Silent failure at industrial scale.
- **AWS S3 outage of Feb 28, 2017.** An engineer ran a debugging command with a typo that took down a larger subset of S3 servers than intended. The command ran cleanly. Public RCA: <https://aws.amazon.com/message/41926/>. Anchor case for "scope was wider than intended."
- **Mat Honan's epic hacking (Wired, August 2012).** Not strictly a terminal incident, but the climactic moment is a `find -exec rm` running on the author's MacBook via remote wipe. He lost a year of his daughter's baby photos. Useful for the irreversibility beat. Documented in Wired's "How Apple and Amazon Security Flaws Led to My Epic Hacking."

---

## 2. The Core Concept — State of the Field

### What is settled

- **AI-assisted task completion ≠ AI-assisted learning.** All three foundational studies converge: helping the user finish faster is not the same as helping the user become more capable. Bastani's exam scores, Kosmyna's EEG, and Anthropic's quiz are three independent measurement modalities producing the same finding.
- **The mechanism is identifiable.** Anthropic's RCT named it most precisely: skill formation depends on *which interaction patterns* the user employs. Delegation patterns produce poor formation; engagement patterns (follow-up questions, code+explanation, conceptual-only AI) produce comparable-to-control formation. This is the empirical foundation under the conducting discipline.
- **Exit 0 is a process-completion signal, not a correctness signal.** This is uncontroversial Unix semantics, not a recent finding. The job of Ch 1 is to make the reader feel the gap, not to argue it.

### What is disputed

- **Are the effects permanent?** Bastani's design measured at one time point; Kosmyna's crossover sessions suggest at least short-term persistence; Anthropic measured immediately. No published RCT yet measures durable skill loss six months later.
- **Does "AI literacy" instruction help?** Bastani's guardrail arm (GPT Tutor with teacher-designed hints) shows that *interface design* helps. Whether *user education* helps is largely untested. The book stakes its argument on operational discipline, not literacy claims — this is the right call empirically.
- **Domain specificity.** All three studies measure non-terminal domains (math, essay writing, Python library learning). The book's claim that terminal AI is *more* dangerous than editor AI rests on argument, not on direct measurement. This is a defensible position (terminal lacks undo, errors are silent, scope can expand) but the author should not pretend it's been measured.

### What has changed recently (last 5 years)

- The empirical literature on LLM-and-learning effectively did not exist in 2021. The Bastani / Kosmyna / Anthropic triplet is the foundation as of 2026. The book can fairly call itself the first practitioner book to *operationalize* these findings for terminal AI.
- The 2026 retirement of `gh copilot suggest/explain/ask` (replaced by interactive `copilot` CLI) does not change the empirical foundation — the patterns Anthropic measured apply across surfaces. But the chapter's specific tool examples must be updated. See Section 8.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 1 sits in the file archiving / log management domain.)

- **The wrong `find -mtime`.** The CLI generates `find . -name '*.log' -mtime +7 -exec mv {} archive/ \;`. The student doesn't notice that `-mtime +7` means *modified more than 7 days ago* and that "modified" includes files touched by build tools as recently as yesterday. Exit 0. Files moved. The build that depended on those logs being in place breaks the next morning.
- **The accidental hidden-file move.** The student asks the CLI to "move all files in `~/projects/old/` to `~/archive/`." The generated `mv ~/projects/old/* ~/archive/` skips dotfiles silently because of glob expansion semantics. The `.git/` and `.env` files stay behind, lose context, and break the project state. Exit 0.
- **The over-eager cleanup.** The CLI generates a script to "remove cache files older than 30 days." The script's pattern matches the student's *Downloads* cache, including a file that wasn't actually cache — a CV draft saved to the wrong folder. The file is gone. There is no undo.

All three pass the AP CS / Mac-or-Linux-laptop accessibility bar.

---

## 4. The Book's Thesis Connection

Ch 1 is where the thesis stops being implicit and becomes explicit. The chapter must do three things:

1. **Name the mechanism.** "Exit 0 ≠ correct" is the chapter's claim, but the *reason* — that pattern completion and scope judgment are different cognitive operations — is what the rest of the book builds on. Without Ch 1's empirical foundation, Ch 4–10's framework can be dismissed as opinion.
2. **Locate the student-supplied capacity.** The Bastani / Kosmyna / Anthropic studies all measure what gets *lost* when the human disengages. Ch 1's job is to show the reader that the lost capacity is the one their teacher will eventually grade them on, the one their employer will eventually pay them for, and — crucially — the one that prevents them from deleting things they can't get back.
3. **Foreshadow the gate.** The Anthropic RCT's high-scoring patterns (follow-up questions, code+explanation, conceptual-only) are operationally indistinguishable from the suggest→explain→verify gate. The chapter does not need to claim this — it should just describe Anthropic's high-scoring patterns and let the reader see, in Ch 4, that the gate is the operationalization.

Specifically student-supplied: scope judgment over the student's own filesystem; intent specification given the student's own goals; consequence assessment given the student's own tolerance for loss.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: William James (1842–1910).** Keep as primary.

Candidates:

- **William James** (1842–1910, USA, psychologist/philosopher). The named figure. His chapter on Habit in *Principles of Psychology* (1890) is the founding statement of habit-as-nervous-system-consolidation. Prompt anchor: "He argued that habit is the nervous system's way of consolidating effortful struggle into automatic capability. The silent failure breaks the mechanism James named." Lesser-known than his other roles (pragmatism, varieties of religious experience) in this specific frame. White, male, American — does not help spread.
- **K. Anders Ericsson** (1947–2020, Sweden/USA, psychologist). Originator of deliberate-practice research. The chapter's claim that "you must struggle through the explain step to build the model" is Ericsson's framework. Less famous to most students. Diversity: white male European, mid-late-20th-century — similar profile to James but different era.
- **Lev Vygotsky** (1896–1934, Russia/Soviet, psychologist). Zone of proximal development. The gate sits in Vygotsky's ZPD — within the student's reach with scaffolding, beyond it without. Diversity: white male, Soviet, early-20th-century — different national/political context. Strongest pedagogical fit; weaker direct fit to "exit 0 vs. correct."

Recommendation: keep James for Ch 1 — the habit-formation frame is exactly what the silent failure breaks. Ericsson is a stronger alternate than Vygotsky here; save Vygotsky for Ch 4 or Ch 11 where scaffolding is the explicit topic.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
The reader needs to know what an exit code is at the level of "0 means the process didn't crash." If they don't, two sentences in the chapter cover it.

### Common misconceptions to disarm
- **"If it worked once it'll work every time."** Pattern completion is stable; scope judgment is context-dependent.
- **"A larger model would have caught this."** No model can read the student's filesystem at the moment of suggestion. Larger models hallucinate more confidently, not less. (Cite Anthropic RCT — model capability did not correlate with skill formation in the trial.)
- **"The numbers don't apply to me — I'm a strong student."** Bastani's GPT Base group was high-school math students at one of Turkey's strongest schools. Strong students lost more, relatively, because they had more to lose.

### Effective instructional sequences
- **Specific-to-general with empirical anchoring.** Open with the friend's archive-script. Generalize to silent-failure taxonomy. Anchor the taxonomy in the Bastani / Kosmyna / Anthropic findings. The numbers land harder *after* the anecdote, not before.
- **Two-path diagram.** TIKTOC already calls for one. The visual sequence (suggest→run→success-apparent→atrophy vs. suggest→explain→verify→understand→capability) is the chapter in a single image.

### Known failure modes
- **Statistic dump.** A chapter that opens with "Bastani et al. found that…" loses the technically-fluent-but-impatient reader before the friend's story can land. Save the numbers for after the anecdote.
- **"Therefore: don't use AI."** The book's whole argument is that the answer is not less use but *disciplined* use. The chapter must not slide into Luddism.
- **Over-statement of measured effects.** Bastani measured GPT-4 with and without guardrails on math problems. The chapter cannot claim Bastani measured terminal work or measured GitHub Copilot CLI specifically.

### What separates understanding from memorization
A reader who understands Ch 1 can articulate, in their own words, *why* delegating command generation without explanation is the specific failure mode the Anthropic study labeled "AI Delegation." A reader who memorized Ch 1 can list the three low-scoring patterns without distinguishing them from each other.

---

## 7. Representation and Display Research

TIKTOC specifies two figures:

- **`<!-- → [TABLE: Silent failure taxonomy — four rows.] -->`** Worked content for each row:

  | Category | What it looks like | What it costs | How to catch it |
  |---|---|---|---|
  | Visible failure | Non-zero exit, error message | Nothing — you noticed | Read the error |
  | Silent wrong scope | Exit 0, more files affected than intended | Data loss; broken downstream tools | `--dry-run` or `-print` before `-exec` |
  | Silent wrong target | Exit 0, right operation, wrong files | Data loss in the wrong place | Verify match list before action |
  | Silent wrong timing | Exit 0, runs at wrong moment in pipeline | Race conditions; corrupted state | Check pipeline order before chaining |

- **`<!-- → [DIAGRAM: The silent failure — two-path diagram.] -->`** Editorial style. Two horizontal sequences, top and bottom, sharing a starting node (`suggest`). Top: `suggest → run → success (apparent) → no understanding → atrophy`. Bottom: `suggest → explain → verify → understand → build capability`. Same starting point, divergent endpoints.

No additional displays required.

---

## 8. Open Questions and Research Gaps

- **CLI version.** Same flag as Ch 0. The chapter's specific examples (`gh copilot explain` invocations) need updating against the post-January-2026 surface. The pedagogical claim survives; the syntax doesn't.
- **Terminal-specific RCT.** None exists. The book argues that terminal AI is more dangerous than editor AI on three structural grounds (no undo, silent errors, scope surprises). This is an argument, not a measurement. The author may want to commission or partner on a follow-up study; for now, flag the claim as "argued, not measured" in a footnote.
- **Bastani numbers in Seth's voice.** Ch 1's opening uses "the Bastani finding stated for terminal work." The author should decide whether to give Seth the literal numbers (127% practice gain / no exam gain / Base group worse than control) or just the directional finding. Recommendation: give the numbers — Seth is a technically fluent reader and the precision lands.
- **Anthropic RCT — domain transfer.** The RCT measured Python developers, not high-school students. The book is using it as the closest available analogue to terminal AI for the target reader. This transfer is reasonable but should be acknowledged in the chapter, not papered over.

Sources likely to need refresh within 3 years: any specific quote of the `gh copilot suggest/explain/ask` syntax. The empirical citations age slowly because they're the founding work.

---

## 9. Sourcing Notes

- **Bastani PNAS 2025**: open access, stable URL.
- **Kosmyna arXiv 2025**: preprint at time of writing. Confirm peer-reviewed publication status at press. If it remains preprint, cite as such — do not pretend it's been peer-reviewed.
- **Anthropic RCT 2026**: blog + arXiv. The blog version is the publicly-cited form; the arXiv preprint contains the methodology detail. Cite both.
- **GitLab 2017 post-mortem**: stable, public, technically dense. Author should read the post-mortem in full before drafting — there are specific details (the backup chain failure modes) that will sharpen the chapter even if not cited.
- **Pixar Toy Story 2 / Mat Honan**: secondary-source-heavy. Anchor to the original Wired piece (Honan) and Oren Jacob's interviews (Pixar). Do not cite blog re-tellings as if they were primary.
- **Risko & Gilbert 2016 / Sweller 1988**: standard academic citations, stable. Useful for one-sentence mechanism support each; not the empirical anchor.
