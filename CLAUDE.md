# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

An Obsidian vault used as a personal "second brain." Content is plain Markdown (`.md`) with optional YAML frontmatter; `.obsidian/` holds the editor config. There is no build system, package manager, or test suite — edits to notes are the work.

## Organizing principle: PARA

All notes live under one of four top-level folders. When creating a note, place it according to *what the user is doing with it right now*, not its topic:

- **Projects/** — Short-term efforts with a defined outcome and end date (e.g. "Write blog post on X", "Plan trip to Y"). Move to `Archives/` when done or abandoned.
- **Areas/** — Ongoing responsibilities with no end date (e.g. "Health", "Career", "Finances"). Maintained indefinitely.
- **Resources/** — Topic-based reference material that isn't tied to a current project or area (e.g. "Books", "Articles", "Tech notes").
- **Archives/** — Inactive items from any of the above. Don't delete; move here.

The same topic can shift folders over time. A "Marathon training" note starts in `Projects/`, moves to `Areas/Health/` if running becomes ongoing, and ends up in `Archives/` if dropped. When a note's status changes, move the file rather than duplicating it.

## Editing conventions

- Default to creating notes in the folder that matches their PARA category. If unsure between Projects and Areas, ask whether there's a defined end state.
- Use `[[wikilinks]]` for links between notes (Obsidian default), not Markdown links, so backlinks work.
- Keep filenames human-readable (`Marathon training plan.md`), not slugified.
- Don't touch `.obsidian/` unless the user asks — it's the editor's state, not content.

## Templates and daily logs

- `Templates/` is a top-level utility folder, not part of PARA — don't reclassify it. Obsidian's core Templates plugin is configured to read from it (`.obsidian/templates.json`).
- Templates use the core plugin's substitutions: `{{date:YYYY-MM-DD}}`, `{{time:HH:mm}}`, `{{title}}`. These resolve at insertion time, not at file creation.
- Daily-log workflow (e.g. `Areas/Mental Health/Logs/`): create a new note in the area's `Logs/` subfolder named `YYYY-MM-DD.md`, then run **Templates: Insert template** from the command palette.
