<!--
    00-frontmatter.md
    FRONT MATTER — everything that appears before Chapter 1.

    This file contains four sections in order:
      1. Copyright page
      2. Dedication (optional — delete if not using)
      3. Preface

    Do not number these sections. They use roman numerals in print
    and appear before the body in the compiled EPUB.
-->

# GitHub Copilot CLI for Students

**Nik Bear Brown** with **Seth Brown**

---

## Copyright

Copyright © 2026 Nik Bear Brown. All rights reserved.

Published by Bear Brown, LLC.

No part of this publication may be reproduced, distributed, or transmitted
in any form or by any means without the prior written permission of the
publisher, except in the case of brief quotations in critical reviews and
certain other noncommercial uses permitted by copyright law.

ISBN: [INSERT ISBN]

---

## Dedication

<!-- Optional. Delete this section if not using. -->

*[For — ]*

---

## Preface

<!-- The preface is written in the author's voice.
     It answers three questions:
       - Why does this book exist? (the gap it fills)
       - Why now? (what changed that makes this urgent)
       - Why you? (what credentials or experience qualify you to write it)
     It is NOT a summary of the book — that belongs in the Introduction.
     Typical length: 2–5 pages. -->

The terminal is where AI-assisted coding is most useful and most dangerous at the same time.

Most useful, because `gh copilot suggest` removes the friction of recalling syntax — the `find` invocation, the `awk` one-liner, the `git rebase` flag — that even experienced engineers stop to look up. Most dangerous, because the terminal has no undo. A command that exits zero and silently does the wrong thing — moves the wrong files, deletes a directory the running build depended on, force-pushes over a teammate's branch — leaves no green checkmark to argue about. The damage is already on disk. The shell flags only failure-to-complete, not failure-to-be-correct, and those are different things.

This book exists because that asymmetry is the operational reason students need a different discipline at the terminal than they need in an editor.

The pattern showed up first in my son's lab — a friend running `gh copilot suggest "archive log files older than 7 days"`, skimming the result for a half second, appending `-exec mv {} archive/ \;`, hitting enter, and watching the prompt come back clean. Three days later the build broke because `-mtime +7` in `find` means files whose *modification time* is more than seven days ago, not files that are unused by the project. The CLI did exactly what it said it would do. The student did not know the difference between "old" in the filesystem sense and "old" in the project sense. The CLI does not know either. The gap between those two senses of "old" is where the silent failure lived.

I teach AI engineering at Northeastern University. I have spent the last decade watching what happens when capable tools enter the loop between a practitioner and their work. The CLI surface is a special case — sharper failure modes, no undo, scope that expands in ways the user does not notice — and the discipline has to be tuned for it. The conducting discipline this book teaches has a specific operational form: never run a `gh copilot suggest` output without running `gh copilot explain` on it first. Read the explain. Form an expectation. Then execute. That is the gate. The rest of the book is what grows around the gate — CLI.md, the five supervisory capacities, the handoff conditions where the agent's plan and the system's actual state diverge.

That is the gap this book fills. GitHub publishes excellent CLI documentation. There are developer-focused workflows for senior engineers who already have the scar tissue that makes them cautious. There is, as far as I can tell, no book yet that takes the terminal's specific failure modes — silence, irreversibility, scope expansion — and teaches a student how to *conduct* the CLI rather than be steered by it. This is that book.

A note on voice. The book is co-written. My son **Seth Brown** is the co-author. Seth is a high-school senior in Troy, Missouri, and a self-taught game developer who works at the terminal every day — Git/GitHub for version control on his game projects, shell scripting for Godot and Roblox build pipelines, Node.js tooling for his Next.js platform *Zebonastic*. He has shipped *Haunt & Harvest* (Godot 4 co-op horror, migrated from Unreal Engine), *Midnight Fuel* (Roblox/Luau horror), and *Bubble Pop* (Google Play arcade with AdMob).

The deeper credential is operational. Seth and I co-built two production AI agents that operationalize the discipline this book teaches — *Walker*, a Unity refactoring conductor that enforces a five-phase audit-restructure-CLAUDE.md-refactor-verify model and names the five supervisory capacities by initial; and *Zelda*, a senior game-design-documentation consultant with a 34-command library, a phase-gate enforcement layer, and a seven-failure-mode audit pass. Both are public at [humanitarians.ai/tools](https://www.humanitarians.ai/tools/). The suggest → explain → verify gate, CLI.md, the five supervisory capacities — the discipline this book teaches at the terminal — shows up in Walker and Zelda as deployable form. The books teach the abstraction; the agents are the worked artifacts that prove the abstraction holds up under production pressure.

The terminal-discipline chapters in this book are written against the constraints of Seth's actual work — the `.godot/imported/` caches, the export artifacts, the export presets, the kinds of accidents that happen when an `rm -rf` runs in the wrong subtree of a game project. The narrative chapters are in Seth's voice. The framework chapters are in mine. The voice shifts on purpose. A discipline written only by adults about how students should behave has one shape. A discipline worked out by a practitioner using the CLI on his own software, with an adult helping articulate the structure, has a different shape — and a different authority.

The book does not cover the full surface of `gh`, every flag in `gh copilot`, or the shipping changes in the new interactive `copilot` CLI. It does not argue whether students should use AI; they will, the question is how. It does not pretend the discipline is finished; the closing chapters say exactly what would change our minds.

If you are a student opening this book, the discipline is empirical. Try it on one terminal session. Run the explain before the suggest output. See what you catch that you would otherwise have missed. The difference is what the book is for.

— Nik Bear Brown and Seth Brown
Boston, Massachusetts and Troy, Missouri
May 2026
