---
name: orca-diff-notes
description: >-
  Pin agent-authored rationale to specific diff lines in an Orca-managed worktree so it
  renders inline beside the code in Orca's diff pane. Use when the user asks for review
  notes or annotations on a changeset, says "add diff notes", "annotate this change",
  "leave rationale on the diff", "explain the change line by line", or wants you to narrate
  a diff you just produced. Distinct from human review notes, which Orca renders without the
  agent badge.
---

# Orca Diff Notes

This file is a discovery stub, not the usage guide. The full, version-matched guide is
served by the Orca binary itself — kept out of this file so it can never drift from the
binary that will actually run your commands.

Pin agent-authored rationale to specific diff lines in an Orca-managed worktree. Use when
the user asks for review notes or annotations on a changeset, says "add diff notes",
"annotate this change", "leave rationale on the diff", "explain the change line by line",
or wants you to narrate a diff you just produced.

## Resolve the CLI

Resolve the executable exactly as the `orca-cli` skill describes: `orca`, or `orca-dev` in a
dev checkout whose session exposes `ORCA_DEV_REPO_ROOT`, or `orca-ide` on Linux outside
Orca terminals. Below, `ORCA` is that resolved executable — substitute it literally.

## Load the full guide before pinning notes

```text
ORCA skills get orca-diff-notes
```

Read it first, then pin notes with `ORCA diff-note create`. Don't guess subcommands or
flags from memory; they change between Orca releases. Prefer `--json` for agent-driven calls.
