# Chapter 10 — When the Build Is Creative: Scripts with Aesthetic Choices

> The terminal is not just for automation. When your script has aesthetic choices — output format, naming conventions, interaction design — the creative judgment stays yours.

---

## Learning outcomes

1. **(Understand)** Explain how the fluency trap manifests in creative terminal work.
2. **(Apply)** Apply the CLI.md creative section to a script that has output formatting choices.
3. **(Analyze)** Distinguish aesthetic judgment (irreducibly human) from mechanical execution (CLI's domain) in a provided terminal build.

---

## Opening

Seth was building a weekly dev-log generator for Zebonastic.

The script read his git commits from the past week across the Haunt & Harvest and Midnight Fuel repos, his Zebonastic article drafts folder, and his to-do file, and produced a one-page markdown dev log. He wrote it to be publishable — a record he posts every week to the Zebonastic site for the developers and educators who read it.

The CLI generated the formatting. Seth used `gh copilot suggest` to compose a `bash` script that took the inputs, organized them, and printed the result. The script worked. The output was correct. The technical implementation was right.

The output had no voice.

The summary was structured ("This week's commits: ..." "This week's drafts: ..." "This week's to-dos: ..."). The structure was reasonable. The summary read like the *average* weekly dev log the CLI had seen in its training data — chipper, generic, vaguely encouraging. It was indistinguishable from a thousand other indie-dev logs that did not exist but that the CLI's training had implied could exist.

When Seth read his summary, it did not sound like him. It sounded like the model. The model is good at the average. Seth is not the average.

This is the **creative version** of the fluency trap from Chapter 1. The technical implementation is right. The output is correct. The *meaning* of the output — its voice, its stance, what choices about format and tone it makes — has drifted to the model's most-probable choice, which is the choice no specific person would have made on purpose.

This chapter is the discipline that keeps creative judgment yours in terminal builds.

---

## The creative chapter, for terminal work

You may not think of terminal scripts as creative work. Mostly they are not. A `find` is not creative. A `git rebase` is not creative. Most of what the book has taught — the gate, the capacities, CLI.md, the formulation discipline — is for *automation* builds where the right answer is determined by the spec.

Some terminal scripts are creative. The boundary is:

- **Automation builds:** the right output is determined. Same input + same script = same output, and the output's properties (correctness, performance) are objective.
- **Creative builds:** the right output requires *human judgment about what would serve the audience*. Same input + same script = same output, but two different correct scripts could produce two different outputs, and the choice between them is yours.

Examples of creative terminal builds:

- A weekly dev-log generator (the chapter opening).
- A script that generates README files for your projects.
- A script that formats error messages for human readability.
- A script that produces release notes from commit history.
- A script that pretty-prints a JSON config file in a way you find readable.
- An interactive script that prompts the user for inputs and structures the conversation.

The common property: a human reads the output. The output's *voice*, *structure*, and *stance* matter to whether the human finds it useful. The CLI can produce *a* correct version. You produce *your* correct version.

---

## What the fluency trap looks like in creative work

The trap has three shapes for creative terminal output.

**Voice drift.** The summary that sounds like the model. Generic, slightly cheerful, hedge-laden. The voice is fine. It is not yours.

**Structural defaults.** The README that follows the most-probable README structure. Headers in the standard order, sections in the standard places, an *Acknowledgments* section because most READMEs have one even if your project does not need one. The structure is fine. It is not the structure that serves your project.

**Stance avoidance.** The error message that says *"Something went wrong. Please try again or contact support if the problem persists."* The model's training data is full of error messages that avoid stating responsibility, that suggest generic next steps, that hedge. Your error message might more usefully say *"The script needs `~/projects/my-project/.env` to exist before running. Create it with the template at `~/projects/my-project/.env.example`."* — specific, responsible, immediately actionable. The model's default is the hedge; yours is the specific stance.

The trap is that the model's output *reads* as fluent. Reading fluent output produces the feeling of "this is fine." For *automation* builds, fine is often enough. For *creative* builds, fine is precisely the failure: the output is fine and forgettable when it should be specific and useful.

---

## The creative section in CLI.md

The remedy is to make your aesthetic decisions explicit, the way you make your technical decisions explicit, and to keep them in CLI.md.

Add a section to CLI.md for the creative-aesthetic decisions:

```markdown
## Voice and aesthetic conventions

### Output voice
- Matter-of-fact. No "great job!" or "happy coding!" or other generic
  encouragement.
- Direct second-person where the output addresses the user.
- No emoji unless the output is structurally part of a UI that uses emoji.
- Specific over generic. Name the file, the count, the action — not "your data".

### Error messages
- State what is wrong specifically.
- State what the user can do to fix it, with the exact command or path.
- Do not say "contact support" or "try again."
- Cite the relevant config file or env var by name.

### Structure
- Summary scripts: short prose paragraph, then a bulleted list of specifics.
  No tables unless the data is naturally tabular.
- README files: one-line description, then "How to use," then "How it works,"
  in that order. Skip "License" unless I have actually licensed it. Skip
  "Acknowledgments" unless there's a specific person to acknowledge.

### Examples
- A weekly dev log: see `~/projects/zebonastic/devlog/2026-04-21.md`
  for the voice and format I want.
- A README I like the voice of: `~/Projects/HauntAndHarvest/README.md`.
```

The section grows over time, the same way the lessons-learned section grows. Each time the CLI's default output drifts from your voice, you note the drift in this section so the next prompt can reference it.

When you write a `gh copilot suggest` prompt for a creative script, paste the relevant lines from this section into the prompt. The CLI's output will fit your voice — not because the CLI suddenly developed taste but because you supplied the taste explicitly.

---

## Worked example: the weekly dev log, twice

Same input. Same script structure. Two prompts. Different outputs.

**Prompt 1 (without creative-section context):**

```
gh copilot suggest "bash script that reads my git log from the past week across my game repos and prints a weekly dev log"
```

The CLI generates a script that produces output like:

```
========================================
This Week's Activity
========================================
Great work this week! Here's a summary of your accomplishments:

📊 COMMITS THIS WEEK: 23
🎯 Top repositories:
   - HauntAndHarvest (12 commits)
   - MidnightFuel (8 commits)
   - zebonastic (3 commits)

Keep up the great work! 🚀
```

The script runs. The output is technically correct. The voice is the model's. The emojis, the "Great work!", the "Keep up the great work!" — none of these are choices Seth would have made.

**Prompt 2 (with creative-section context pasted):**

```
gh copilot suggest "bash script that reads my git log from the past week across my game repos and prints a weekly dev log, in the voice from the Voice and aesthetic conventions section of my CLI.md: matter-of-fact, no generic encouragement, no emoji, direct second-person, specific over generic"
```

The CLI generates a script that produces output like:

```
Week of 2026-05-18 — git activity

23 commits across 3 repositories:
- HauntAndHarvest: 12 commits, last on 2026-05-23
- MidnightFuel: 8 commits, last on 2026-05-22
- zebonastic: 3 commits, last on 2026-05-19

The HauntAndHarvest repo has the most activity. The largest commit
(by lines changed) was the inventory refactor on 2026-05-21.
```

Same CLI. Different output. The 100 words of creative-section context in the prompt are why.

The output above is the dev log itself — terse, factual, written for builders. The Zebonastic articles the dev log accompanies share that register. A typical opener from Seth's published archive:

> Start with a number that shouldn't be possible: 287,000. That's how many people were playing *The Elder Scrolls V: Skyrim* simultaneously at its 2011 launch peak. Fine — new release, expected. The number that's actually strange is the one that comes fourteen years later: roughly 1,400 people logged in on an average day in 2025. Not a remaster. Not a sequel. The same game, still breathing.

Or another, from a piece on AI-generated music:

> You have 568,707 SEC Form D filings on one end of a research pipeline. On the other end: a horror game that makes you check over your shoulder in your own apartment. These two facts are not as far apart as they seem. Both involve the same problem — signal extraction from noise.

The shape repeats. A specific number. An immediate framing of what makes the number worth thinking about. A refusal to use marketing voice or to soften the analysis with generic encouragement. The CLI cannot invent this voice — it has no way to know that "287,000" and "568,707" are the right places to start, or that the second-person address ("you") is for builders rather than consumers. The CLI can be told the voice exists, named, with example files, and instructed not to overwrite it. The *Voice and aesthetic conventions* section in CLI.md is that telling.

**The lesson:** the model's defaults are the model's defaults. You replace them with yours by stating yours explicitly. The discipline scales the same way as for automation builds — supply the context the model does not have.

**The limit:** the output is still produced by the model. It will read more like you than like the model's default, but it will not read like you in the way that something you wrote yourself would. For outputs that need to be in *your full voice* (a personal letter, a piece of writing for class, anything where you are the author), the model's output is the draft and your revision is the published version. The same is true for code that has aesthetic dimensions — Codex generates the implementation; you revise the variable names and the comments to your voice.

---

## When *not* to use the discipline

Not every script needs a voice. A `cron` job that deletes old temp files does not need a voice. A `find` that produces a list of paths does not need a voice.

The heuristic: **does a human read the output?** If yes, voice matters. If no, voice does not. A script that pipes its output into another script's stdin is producing data, not voice; the discipline does not apply. A script that prints a summary to your terminal at the end of the day is producing a thing you will read; the discipline applies.

A subtlety: error messages always have a human reader, even when the script's main output does not. The error message that fires when the script fails is the moment you (or your future self, or a collaborator) will read at 11 PM trying to figure out what went wrong. The voice of error messages matters even for scripts whose normal output does not.

---

## Common misconceptions

**"Creative work is for writers; my scripts are technical."** Most of the book's chapters cover automation builds where this is mostly true. This chapter is about the cases where it is not. Even technical scripts produce output that humans read — error messages, status updates, end-of-run summaries. Voice applies there.

**"The model's voice is fine."** Sometimes. The chapter is for cases where fine is not enough — where the output exists to serve *you* and *your audience*, and where the model's average voice does not serve either.

**"This is too subjective for a discipline."** The discipline is not "be more aesthetic." The discipline is "make your aesthetic decisions explicit in CLI.md and paste them into prompts." The making-explicit is operational. The aesthetic decisions themselves are yours.

**"I'll just edit the output afterward."** You can. The cost of editing the model's default output to your voice is usually higher than the cost of generating output in your voice from the start, because the model's structure shapes the editing more than you would expect.

**"This is only for student-scale projects."** No. Senior engineers maintain conventions for their CLI tools' outputs precisely because the cost of off-voice output compounds — in onboarding, in documentation, in the moments when the team's tooling needs to feel coherent rather than assembled-from-defaults.

---

## Exercises

1. **(Apply)** Add a *Voice and aesthetic conventions* section to your CLI.md. Six entries. Cover at least output voice, error messages, and structure.

2. **(Analyze)** Take a script you have written (or generated) that produces human-readable output. Read three runs of the output. Identify two places where the voice does not match what you would have written. What would your version say differently?

3. **(Create)** Rewrite the weekly dev-log script (or an analogous creative script of your own) with the *Voice and aesthetic conventions* section pasted into the prompt. Compare the new output to the original. Was the difference what you expected?

---

## What would change my mind

The chapter's central claim is that **explicit aesthetic conventions in CLI.md produce materially more voice-faithful output** from `gh copilot suggest` than free-form prompts. The claim is plausible from prompt-engineering literature but not directly measured for terminal work. If a controlled comparison found that students who maintained the creative section produced output no more voice-faithful than students who did not, the section becomes optional. The chapter would still recommend explicit aesthetic decisions for the supervisory practice; the case for the dedicated CLI.md section weakens.

The chapter operates on the observation — anecdotal but consistent — that the CLI's output shifts substantially when voice context is pasted, and that the shift is in the direction of the pasted context.

---

## Still puzzling

- **The threshold above which the creative section is worth maintaining.** A student who writes one creative script per semester probably does not need the section. A student who writes one per week does. Where the threshold is exactly varies.

- **Whether the model's voice will become harder to override as models get larger.** Plausible argument: larger models have stronger defaults that take more prompt context to overcome. Plausible counter-argument: larger models follow instructions better and take less context to overcome defaults. Empirically open.

- **How to teach voice without teaching a specific voice.** The chapter teaches that you should have explicit voice conventions, not what those conventions should be. Whether students can find their own voice through the discipline, or whether the discipline produces a generation of students with similar-sounding scripts (because they read the same examples), is open.

---

## AI Wayback Machine

🕰️ **Sol LeWitt** (1928–2007) — American conceptual artist whose *Paragraphs on Conceptual Art* (1967) argued that *the idea is the work* — that the person who holds the intent and writes the instruction is the author, regardless of who executes.[^1] LeWitt's wall drawings were instructions: a set of constraints and operations that anyone competent could execute. The execution varied; the work was the same, because the *concept* was the work. The instructions were specific enough that the executor's voice did not drown the artist's; LeWitt's voice was in the constraints.

The creative section in CLI.md is LeWitt's discipline applied to terminal output. You specify the constraints — voice, structure, stance — completely enough that the CLI's execution does not drown your authorship. The CLI is the executor; you are the author. The instructions in CLI.md are your wall-drawing constraints. The output is yours regardless of who typed the script. LeWitt argued, more than fifty years before this chapter, that the human who holds the constraints is the author. The chapter is his argument applied to a tool he could not have anticipated.

---

## Bridge

You have the full conducting discipline. Chapter 11 is the planning phase of your first complete shell project — built end-to-end with everything the book has taught.

---

[^1]: LeWitt, S. "Paragraphs on Conceptual Art." *Artforum* 5, no. 10 (1967): 79–83. See also LeWitt's "Sentences on Conceptual Art" in *0–9*, no. 5 (1969).
