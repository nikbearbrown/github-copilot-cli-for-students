# Research: Chapter 0 — Introduction: The Cautious Builder
## GitHub Copilot CLI for Students: A Practitioner's Guide

**Chapter one-line:** Meet Seth. He noticed something his friends didn't — that exit 0 is not the same as correct.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Wiener, Norbert. *The Human Use of Human Beings: Cybernetics and Society*. Houghton Mifflin, 1950 (rev. 1954).** The opening chapter's intellectual lineage. Wiener's central question — what does a tool do to the human who uses it? — is exactly the question Seth is asking about `gh copilot suggest`. Contributes the moral framing of Chapter 0 without leaning on the term "cybernetics" in the body.
- **Carr, Nicholas. *The Shallows: What the Internet Is Doing to Our Brains*. Norton, 2010.** The closest popular treatment of cognitive offloading prior to LLMs. The terminal version of Carr's argument — the tool that makes you faster also makes you weaker — is what Seth feels before he can name it. Use for the "I felt it before I had vocabulary" beat.
- **Brynjolfsson, Erik & Andrew McAfee. *The Second Machine Age*. Norton, 2014, ch. 12.** Establishes the "complement vs. substitute" framing for AI tools. The chapter can borrow this distinction without citing it directly: the friend treats the CLI as a substitute, Seth treats it as a complement.
- **Risko, Evan F. and Sam J. Gilbert. "Cognitive Offloading." *Trends in Cognitive Sciences* 20, no. 9 (2016): 676–688.** The canonical cognitive science review. Names the mechanism Seth is observing. Useful for one technical sentence in Ch 0 without sending the reader to the literature.

### Key empirical cases

- **The Toy Story 2 near-deletion incident (1998).** Pixar lost ~90% of the in-progress Toy Story 2 files when an animator's `rm -r -f *` ran on the wrong directory. Recovered only because a technical director had a personal backup at home for maternity leave. Documented in *The Pixar Touch* (Price, 2008) and Oren Jacob's interview, "Pixar's Toy Story 2 Was Deleted Twice, Once by Accident" (Fast Company, 2012). Real, terminal, irreversible. Ideal cold-open precedent for the silent-failure thesis. **Do not name in Ch 0 itself** — save for Ch 1 or Ch 9. But it should sit in the author's back pocket.
- **The GitLab database deletion incident (Jan 31, 2017).** YP-1, an engineer, ran `rm -rf` on what he thought was a secondary database; it was production. Five of five backup methods had silently failed. Documented in GitLab's public post-mortem. The point that resonates with Seth's friend's story is not the dramatic deletion but the *silent* failure of the safety nets — backups that exited 0 every night while writing nothing.
- **Seth's friend's archive-script incident (illustrative, anchored to Seth's testimony).** The file-processing script that ran on every file instead of only new ones. Mark explicitly as a Seth Brown narrative source — the author should confirm with Seth that the details (which directory, what was processed) are reproducible in publication form.

---

## 2. The Core Concept — State of the Field

### What is settled

- Cognitive offloading is real and measurable (Risko & Gilbert 2016). Tools that take over a cognitive task durably reduce the user's independent ability to perform it, at least in the absence of deliberate practice.
- For LLM-assisted intellectual work specifically, controlled experiments now show learning loss without guardrails (Bastani et al., PNAS 2025) and reduced neural engagement (Kosmyna et al., arXiv 2025). Both findings transfer directly to terminal AI: there is no reason the terminal would be exempt.
- Exit codes are a contract about whether the *process* completed without error, not whether the *task* was correct. This is in every Unix textbook (Kernighan & Pike, *The Unix Programming Environment*; Stevens, *Advanced Programming in the Unix Environment*).

### What is disputed

- Whether learning loss is permanent or reversible after a period of unaided practice. The Bastani guardrails arm suggests reversibility is possible *during* learning if guardrails are present; the Kosmyna crossover sessions suggest at least short-term persistence of weakened engagement.
- Whether "AI literacy" curricula change behavior or just performance on AI-literacy tests. The book should not stake its argument on AI-literacy claims — stake it on the operational discipline instead.

### What has changed recently (last 5 years)

- LLM-assisted command generation in the terminal is now a default workflow for the target reader, not an experimental tool. The relevant question shifted from "should students use it?" to "how should they use it?"
- **The `gh copilot suggest/explain/ask` extension was retired in January 2026** and replaced by a new interactive `copilot` CLI with plan/autopilot modes (docs.github.com/copilot). The pedagogy in this book survives the change — the suggest→explain→verify *gate* is tool-agnostic — but Ch 0's specific tool naming will need a one-paragraph version note. See Section 8.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 0 sits in the file archiving / log management domain.)

- **Log rotation gone wrong.** Student writes a script to archive `*.log` files older than 7 days. The `find` glob also catches `.log` files inside `node_modules/` directories the student never intended to touch. Exit 0. Files moved. Build broken later because tooling expected those logs in place.
- **Photo-import "cleanup."** Student asks the CLI to "delete duplicates from the Downloads folder." The CLI generates a `find` command that compares by filename, not content. Real duplicates with different names stay; legitimate files with collision names disappear. Exit 0.
- **Homework directory archival.** Student asks for a command to "archive last semester's work." The command moves the contents but also moves the `.git/` directory of an in-progress project that lived in the same parent. Exit 0. The student loses two days of uncommitted work.

All three are accessible to an AP CS student, none require domain expertise beyond "I have a Mac and a Downloads folder."

---

## 4. The Book's Thesis Connection

Ch 0 is the *felt* version of the thesis — the reader meets the silent failure as a story, not as an argument. The thesis ("exit 0 is not the same as correct; the conducting discipline is the difference between building real capability and atrophy") is whispered, not stated. The job of Ch 0 is to make the reader feel the cost of running generated commands without explanation before any framework appears.

What the chapter must establish, on the thesis's behalf:
- The terminal raises stakes that the editor doesn't (no undo, silent errors, scope surprises).
- "AI is generating commands my friends can't explain" is a current, observable reality, not a hypothetical.
- The supervisory work the book will name (PA, PF, TO, IJ, EI) cannot be transferred to the tool. This claim is foreshadowed only — the chapter does not yet defend it.

What only the student can supply (and the CLI cannot):
- Whether *these* files are the ones to archive (scope judgment over their actual filesystem).
- Whether deletion is reversible *in their actual environment* (no remote backup vs. cloud-synced).
- Whether the consequence of getting it wrong is acceptable to *them* (a school assignment vs. a year of personal photos).

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Norbert Wiener (1894–1964).** Keep as primary.

Candidates:

- **Norbert Wiener** (1894–1964, USA, mathematician/cybernetician). The named figure. Wikipedia-accessible. Famous-tier — fails the "lesser-known preferred" rule but satisfies on substantive fit: Wiener's *Human Use of Human Beings* is the single clearest statement of "what does the machine do to its user?" Prompt anchor: the 1950 first edition. Diversity: white, male, American, mathematical — does not help the spread.
- **Mary Allen Wilkes** (born 1937, USA, computer scientist). First person to use a personal computer in their home (LINC, 1965). Less famous, satisfies diversity (woman). Substantive connection is thinner — she's about access, not about cognitive cost. Use only if the spread across the full set demands a woman in Ch 0 and a stronger fit can be found elsewhere.
- **J. C. R. Licklider** (1915–1990, USA, psychologist/computer scientist). "Man-Computer Symbiosis" (1960). Licklider explicitly asked which cognitive work belongs to the human and which to the machine — closer to Ch 5's framing than Ch 0's, but he is also a strong Ch 0 candidate because his answer ("partnership") is what Seth is groping toward. Lesser-known than Wiener. Same diversity profile as Wiener.

Recommendation: keep Wiener for Ch 0. If diversity audit across the full 15 chapters demands rebalancing, Wilkes is a candidate substitute here — though Licklider in Ch 5 may be the bigger swap target.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
Terminal experience at the cd/ls/mkdir/git level. The chapter must not assume the reader already knows what `gh copilot suggest` *does*; the three-tool introduction belongs in this chapter, not in Ch 4.

### Common misconceptions to disarm
- "If the command worked, it was right." (The book's thesis. Disarm via the friend's story.)
- "AI suggestions are safer than my own commands because they're trained." (Cite the Bastani / Kosmyna / Anthropic triplet *briefly* — full treatment is Ch 1.)
- "Terminal mistakes can be undone." (Name three commands that can't: `rm -rf`, `git push --force` to a shared branch, `find -exec rm`.)

### Effective instructional sequences
- **Pebble-in-the-pond opening.** The chapter does this already by design — Seth observing the friend before the framework is named. This sequence (concrete failure → felt need → framework) is the standard pedagogical move for self-directed adult learners (Knowles, *The Adult Learner*) and works especially well for technically fluent novices who resist top-down instruction.
- **Concrete-to-abstract.** Lead with the script-that-processed-too-many-files. Generalize to "silent failure" only after the reader has the image in their head.

### Known failure modes
- The "just give me the commands" reader (TIKTOC's Adoption Risk #1). The chapter must not feel like a lecture. Seth's voice carries the ethics; the framework comes later.
- Sounding moralistic about terminal safety. The friend is not foolish — the friend is what most students currently are. The book must not let the reader feel superior in Ch 0 or they'll skip the discipline in Ch 4.

### What separates understanding from memorization
A reader who *understands* Ch 0 can name, in their own life, a moment when a command exited 0 and they were uncertain whether it did what they meant. A reader who *memorized* Ch 0 can repeat "exit 0 isn't correctness" without producing an instance.

---

## 7. Representation and Display Research

TIKTOC specifies one figure for this chapter:

- **`<!-- → [DIAGRAM: Seth's arc from observer to practitioner — two-point timeline showing "watches friends" → "builds the discipline". Minimal. Editorial style. No color.] -->`**

Source material for the diagram:
- Two points on a horizontal line. Left: "Seth observes a friend run a generated command and skip explain. Days later: silent wrong scope." Right: "Seth runs his own first fully conducted build. Every command explained before execution."
- Editorial style means thin black rule, type-driven, no decorative axes.
- The diagram should not yet include the framework labels (PA/PF/TO/IJ/EI) — those arrive in Ch 5.

No table is required for Ch 0.

---

## 8. Open Questions and Research Gaps

- **CLI version question.** The book's foundational tool naming (`gh copilot suggest`, `gh copilot explain`, `gh copilot ask`) was retired Jan 2026. The author must decide before Ch 0 drafting whether the book teaches the (still-functional-via-the-new-CLI but renamed) commands, the new interactive `copilot` flow, or both. Recommendation: open the book with a one-paragraph version note that names the retirement, then teach the gate using whichever surface is current at press time. The conducting discipline survives either way.
- **Seth's friend incident — publication form.** The script-that-archived-everything story is told briefly. For publication, the author must confirm with Seth: which OS, which shell, what specifically went wrong, what was lost, what was recoverable. If the story has been told in interviews or notes that exist in `_working/`, link them.
- **Statistical specificity.** Ch 0 mentions "the Bastani finding stated for terminal work" — but the chapter is the felt version, so the numbers ideally don't appear here. Confirm with author whether numbers stay in Ch 0 at all (recommend: no, save them for Ch 1).

Cases marked as illustrative: the three application-domain examples in Section 3 and the friend's archive-script incident are anchored to Seth's testimony or generalized class patterns, not to publicly documented post-mortems.

Sources potentially aging within 3 years: any direct quote of GitHub Copilot CLI syntax. The book's own pedagogy ages slowly.

---

## 9. Sourcing Notes

- **Bastani et al. PNAS 2025** is open-access; cite as PNAS 122 (2025), <https://www.pnas.org/doi/10.1073/pnas.2422633122>.
- **Kosmyna et al. 2025** is arXiv preprint (arXiv:2506.08872, MIT Media Lab). At press time, check whether peer-reviewed version has appeared.
- **Anthropic skill-formation RCT (2026)**: anthropic.com/research/AI-assistance-coding-skills + arXiv:2601.20245. As of May 2026 the preprint is current; check for peer-reviewed version.
- **GitLab 2017 post-mortem** is canonical and stable, but verify URL hasn't moved before press.
- **Pixar Toy Story 2 incident** has been re-told across many secondary sources; the primary anchor is Oren Jacob's on-record interviews, not the more dramatic blog retellings. The Fast Company piece and *The Pixar Touch* (Price 2008, p. 235ish) are the cleanest sources.
- Seth's narrative testimony is a primary source; the author should treat it as such (interview, transcript, or co-authored draft) rather than as a "things Seth said."
