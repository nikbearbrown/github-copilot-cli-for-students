# Chapter 6 — CLI.md: Your Terminal Constitution

> CLI.md is the file you maintain and paste from. It is the difference between a `gh copilot suggest` session that knows your project and one that guesses.

---

## Learning outcomes

1. **(Understand)** Explain what CLI.md is, what it contains, and why it has no automatic injection mechanism.
2. **(Apply)** Write a CLI.md for a student terminal project.
3. **(Analyze)** Distinguish CLI.md content (persistent project knowledge) from command-level context (pasted per invocation).

---

## Opening

Seth opens his second `gh copilot suggest` session on the same project.

The CLI has no memory of yesterday. Yesterday Seth spent thirty minutes typing context into prompts — the project lives at `~/projects/my-project/`, the active logs are appended-to constantly so don't touch them, the `node_modules/` should never be archived, the deployment is to a specific server with an old `rsync` configuration. By the end of yesterday's session the CLI's suggestions were good. Today the CLI is back to default. Seth types the project's directory layout into his first prompt of the day. By prompt three he is repeating yesterday's context almost verbatim. By prompt five he realizes he has spent twelve of his thirty Monday-night minutes telling the CLI things he told it yesterday.

This chapter ends that.

The file is **CLI.md**. It is a markdown file you maintain in your project directory. It holds the persistent context — the things the CLI does not know about your project that it would need to know to be useful. It is where the rules live, where the lessons learned accumulate, where the `never do X` constraints get recorded.

CLI.md has one important property that distinguishes it from the equivalent files in other agentic tools (AGENTS.md for Codex, CLAUDE.md for Claude Code): **CLI.md is not automatically loaded by `gh copilot`.** You paste from it manually into each session's prompts. The lack of auto-injection is, for this book, a *feature* — it forces you to exercise tool-orchestration (Chapter 5's TO) on every invocation. You decide what context to give the CLI each time. The CLI does not auto-default to something you did not consciously specify.

<!-- → [DIAGRAM: CLI.md in the workflow — created before build, consulted before each gh copilot suggest invocation, updated after each session. Contrast: without CLI.md (CLI guesses) vs. with CLI.md context pasted (CLI knows the project). Editorial style.] -->

---

## What CLI.md is

A markdown file at the root of your project. The conventions:

- **Filename:** `CLI.md` (uppercase initials, lowercase extension). Place it at the project root.
- **Size:** under 200 lines. The operational ceiling beyond which the file becomes hard to maintain and hard to paste from.
- **Sections:** the four-section format below.
- **Version control:** commit it to git. The file is part of your project.

The four sections:

**1. Project overview.** One paragraph: what this project does, what the script(s) automate, what is the desired output. The orientation any teammate (or future-you) would need in thirty seconds.

**2. Environment facts.** OS, shell, key directories, important tool versions, anything about your machine or your server that affects how commands behave. The school server's old mod_rewrite. The grading-laptop without Node. The Time Machine backup mount that should never be touched.

**3. Command conventions and exclusions.** The project's rules. *Always* use `--dry-run` before destructive operations. *Never* touch `~/grade-book.csv` with any automation. *Prefer* `find` over `ls` for file enumeration (because of how the project handles spaces in filenames). The "never do X" rules are the most important entries — they are the constraints the CLI cannot infer.

**4. Lessons learned.** Each entry: a date, the mistake, the fix. This section grows over time. The first session's CLI.md has this section empty; by month six, this section is the most valuable part of the file.

---

## Why there is no automatic injection (and why this is good)

The Codex tool reads `AGENTS.md` automatically. The Claude Code tool reads `CLAUDE.md` automatically. The `gh copilot` tool does not read `CLI.md` automatically. There is no mechanism, as of the writing of this book, for the `gh copilot` toolchain to ingest a per-project context file at the start of a session.

You might read this as a limitation of `gh copilot`. The book reads it as a feature, for pedagogical reasons.

**Manual paste forces TO on every invocation.** The Tool Orchestration capacity (Chapter 5) is the work of deciding what context to give the CLI each time. When `AGENTS.md` is auto-loaded, this decision is delegated to the tool — the tool always loads it, regardless of whether the current task needs the loaded context or whether a leaner context would produce better output. When `CLI.md` is manually pasted, you have to *choose* which sections are relevant for the current invocation. The choice is a small exercise of supervisory capacity.

**You see what context the CLI has.** With auto-loaded context, you have to inspect the loaded context to know what the CLI is reasoning over. With manually-pasted context, the prompt you typed is the context — what you see is what the CLI sees.

**You cannot accidentally leak context across sessions.** Auto-loaded context persists. If yesterday's `AGENTS.md` had a rule that no longer applies, and you forgot to update it, the rule still informs today's output. With manual paste, yesterday's lapsed rule is one paste away from gone — if you do not paste it today, the CLI does not see it.

The trade-off: manual paste is slower. You type more. You decide more often. The cost is the discipline tax. The benefit is the supervisory practice.

The book's view: for student-scale builds, the discipline tax is worth paying. The student who pastes from CLI.md every session builds the TO capacity. The student who has auto-loaded context skips the practice and may not notice until something the auto-load did not handle goes wrong.

---

## The four-section CLI.md, worked example

A real CLI.md for a class-website-deployment project.

```markdown
# CLI.md — AP CS Class Website Deployment

## Project overview

Static class website (HTML/CSS/JS, no build step). Source files live in
`~/projects/class-website/src/`. Deployed to the school server via
`~/projects/class-website/deploy.sh` (calls rsync). Site is the primary
reference for the class — broken site = broken week.

## Environment

- OS: macOS 14 (Sonoma)
- Shell: zsh (default)
- Key directories:
  - Source: `~/projects/class-website/src/`
  - Build output: `~/projects/class-website/dist/` (gitignored)
  - Deploy script: `~/projects/class-website/deploy.sh`
  - School server (read-only reference): mounted at `/Volumes/school-server/`
- Tools:
  - `rsync` (system default, version 2.6.9 on the school server — old, no
    `--delete-excluded`)
  - `git` (system default)
  - No `npm` install allowed by the school's machine — pure-shell only
- IMPORTANT: My grade book lives at `~/grade-book/grades.csv`. NEVER touch
  it with any class-website automation. (See lessons learned 2026-03-12.)

## Command conventions

- Always: use `--dry-run` flag (or `echo` prefix, or `find -print` before
  `find -exec`) for any command that modifies the filesystem.
- Always: run `gh copilot explain` on any `find`, `rm`, `mv`, or `rsync`
  command before executing.
- Never: touch anything in `~/grade-book/` from any class-website script.
- Never: use `rsync --delete` against the school server (its rsync is old
  and the behavior is unpredictable).
- Never: push directly to the `main` branch of the class-website repo; use
  a feature branch and PR myself.
- Prefer: explicit paths over `*` globs when scope matters.
- Prefer: `find -print | xargs -I {} cmd {}` over `find -exec cmd {} \;`
  (faster, easier to dry-run by changing `xargs` to `echo`).

## Lessons learned

- 2026-03-12: Accidentally moved old assignment files into the website's
  `dist/` directory. Cause: the `find` glob matched files outside the source
  directory because I ran the command from `~/projects/`, not from inside
  `src/`. Fix: always `cd` into the working directory before running file-
  modification commands; never rely on relative paths from above. Rule
  added to Command conventions.
- 2026-03-19: `gh copilot suggest` generated `rsync -av --delete` to push
  changes to the school server. The `--delete` removed files on the server
  that I had intentionally placed there manually but that were not in my
  local `src/`. Fix: never use `--delete` with the school server's rsync.
  Rule added.
- 2026-04-02: When deploying after a CSS-only change, `rsync` re-uploaded
  the entire site because of mtime drift. The site was unavailable for ~30
  seconds. Fix: use `rsync -c` (checksum-based) for CSS-only deploys. Add
  to deploy.sh as a flag option for next iteration.
```

Roughly 40 lines. Five "never" rules. Three lessons-learned entries. Project overview in one paragraph. The file is short enough to read in two minutes and useful enough that pasting from it changes every `gh copilot suggest` session in this project.

---

## How to paste effectively

You will not paste the whole CLI.md into every prompt. Pasting the whole file every time defeats the purpose; the prompt becomes too long and the relevant rules get diluted by irrelevant context.

The discipline: **paste the sections that are relevant to the current command**.

For a `find` to enumerate files: paste the Environment section's directory layout and the Command conventions section's `find` rules. Skip the rest.

For a `git` operation on the website repo: paste the Command conventions section's never-rules about `main` and PRs, plus any lessons-learned entries about git in this project. Skip the deployment-server context.

For a deploy: paste the deployment-related Environment facts and the lessons-learned about deployments. Skip the source-directory layout.

The choice is the supervisory work. It is also fast — after a few sessions, you know which sections matter for which kinds of tasks, and the paste is mechanical.

A pattern that works: copy the CLI.md sections you need into a scratch file at the top of each session; reference the scratch file as context in your suggest prompts; close the scratch file when the session ends. The scratch file becomes the per-session context manifest.

---

## Updating CLI.md after every session

The lessons-learned section is the most valuable part of the file, and it only stays valuable if you update it.

The discipline: at the end of every session that touched something significant, take two minutes to add a lessons-learned entry. Date, mistake (or near-mistake — count the catches as lessons too), fix, rule added.

The bar for "significant" is low. Did the CLI generate something the explain step caught? That is an entry. Did a `find` glob expand wider than you expected? That is an entry. Did you notice a flag's behavior was different from what you thought? Entry.

The two minutes are the upstream investment that protects every subsequent session. By the end of the semester, the lessons-learned section is the most useful reference for the project — better than any official documentation, because it is calibrated to *your* mistakes.

The two minutes are also where you put what you learned into a form that future-you can use. The cognitive event of writing the entry — articulating what happened, naming the fix — is itself a consolidation step. The lessons-learned section grows; your *capacity* to anticipate similar issues grows alongside.

---

## CLI.md vs. command-level context

A distinction worth being precise about.

**CLI.md is persistent project knowledge.** Rules, conventions, environment facts, lessons learned. The things that are true *every session* in this project. Stable. Versioned in git. Slowly accumulating.

**Command-level context is session-specific.** "I'm currently working in `src/components/`," or "I just rebased onto the latest main," or "I have uncommitted changes in `index.html` that I want to preserve." The things that are true *this session* and that the next session might not need.

The boundary: if you find yourself adding something to CLI.md and your second thought is "this is just for this week" — it does not belong in CLI.md. It belongs in your current prompt or in your session notes. The file is constitution-shaped, not agenda-shaped.

A test: if CLI.md grows past 200 lines, you have been putting agenda-shaped things into it. Prune.

<!-- → [TABLE: CLI.md include/exclude — two columns. Include: environment facts, project-specific conventions, known dangerous patterns, lessons from failures. Exclude: general shell knowledge the CLI already has, constantly changing state, personal notes unrelated to the project.] -->

---

## Common misconceptions

**"CLI.md is documentation for the CLI to read."** It is, eventually, when the CLI gets auto-injection. Until then, CLI.md is documentation for *you* to read and paste from. The CLI reads what you paste.

**"More content is better."** No. The 200-line operational ceiling is real. A focused 80-line CLI.md beats a 500-line one that contains everything you could think of.

**"CLI.md is for big projects."** Even a one-script project benefits. The script will be modified later; the rules will compound; the lessons-learned will accumulate. Start CLI.md at minute one of the project, even three lines.

**"I'll update CLI.md when I have time."** The end-of-session two-minute review is when. If you do not do it then, you will not do it. The lessons-learned entries will be lost.

**"CLI.md is the same as AGENTS.md or CLAUDE.md."** Conceptually similar; mechanically different because of auto-injection. The other tools' files are read by the tool; CLI.md is read by you. The discipline this difference produces is part of the book's argument.

---

## Exercises

1. **(Apply)** Write a CLI.md for a current terminal project. Four sections. Under 200 lines. Commit to git.

2. **(Analyze)** Run `gh copilot suggest` without CLI.md context, then with the relevant sections pasted, on the same task. Document three specific differences in the generated commands.

3. **(Evaluate)** After one week of use: what did you add to CLI.md that you didn't know to include on day one? What does that tell you about how the lessons-learned section will grow over the semester?

---

## What would change my mind

The chapter's strong operational claim is that **CLI.md materially improves `gh copilot suggest` output quality** when used with discipline (relevant sections pasted per invocation). If a controlled comparison — same project, same prompts, with and without CLI.md context — found no measurable difference, the file becomes a personal-organization tool rather than an output-quality tool. The book would still recommend it for the organizational benefit; the case for the discipline overhead would weaken.

The chapter also stakes a pedagogical claim: that **manual paste is better than auto-injection** for student-scale work, because it forces TO exercise. If the data showed that students with auto-injected context (using a tool like Codex's AGENTS.md instead of CLI.md) developed equivalent or better TO capacity over time, the manual-paste argument softens. The chapter would still teach CLI.md as the `gh copilot` file; the pedagogical justification would change.

Neither is directly measured. The book operates on the structural arguments.

---

## Still puzzling

- **When `gh copilot` adds auto-injection (if it does).** GitHub may eventually ship a `CLI.md` auto-load mechanism similar to Codex's AGENTS.md. If they do, the chapter's pedagogical framing changes. The book's working answer is that even with auto-load, the *discipline* of writing and maintaining CLI.md is the value; the auto-load just removes the paste step.

- **Sharing CLI.md across teams.** A project with multiple developers has the question of whether one CLI.md serves everyone, or whether each developer has a personal CLI.md plus a shared one. The book's primary reader is the individual student; team CLI.md is a future-edition topic.

- **The `[private]` section pattern.** Some CLI.md entries — server names, partial credentials, things you do not want in the project's public git history — belong in a private file. The pattern is a separate `~/.cli-private/<project>.md` that you reference but do not commit. The book mentions but does not develop this; it is a more advanced practice.

---

## AI Wayback Machine

🕰️ **Donald Knuth** (born 1938) — computer scientist who created TeX and articulated the principle of **literate programming**: that a program should be written for humans first, with the code as a side effect, so that the human reader and the machine reader are served by the same artifact.[^1] In Knuth's 1984 *Literate Programming*, he wrote: *"Instead of imagining that our main task is to instruct a computer what to do, let us concentrate rather on explaining to human beings what we want a computer to do."* CLI.md is literate programming applied to AI-assisted terminal work. The artifact you write — the file's four sections, the lessons-learned entries, the never-rules — is read by you (for paste-source and reference), by your future collaborators (when you share the project), and eventually by the CLI (when you paste from it). The discipline of writing for *all three readers at once* is what makes CLI.md valuable. Knuth's 1984 argument was that the human-readable explanation is the primary artifact and the executable code is the by-product. The book's argument is the same shape, scaled down to a terminal-AI workflow.

---

## Bridge

You have CLI.md. The CLI knows your project (when you paste). Chapter 7 teaches the discipline upstream of the suggest prompt: problem formulation.

---

[^1]: Knuth, D. E. "Literate Programming." *The Computer Journal* 27, no. 2 (1984): 97–111.
