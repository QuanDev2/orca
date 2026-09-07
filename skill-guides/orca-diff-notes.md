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

Orca renders agent-authored notes inline beside the diff hunk they explain, with an agent
badge and an optional Rationale block. You (the agent) write them from the shell after
making a change; the human reviews them in Orca's diff pane. There is no interactive TUI to
run — notes are created with the Orca CLI only.

Resolve the CLI once (`orca`, `orca-dev` in a dev checkout, `orca-ide` on Linux outside
Orca terminals) exactly as the `orca-cli` skill describes. Below, `ORCA` is that resolved
executable — substitute it literally, never run `ORCA` as a shell variable.

## Workflow

```text
1. Implement the change.
2. Inspect the diff:  git diff -- <file>   (or `git status` for the file list)
3. For each changed line worth explaining:  ORCA diff-note create ...
4. Open the diff for the user:  ORCA file diff <path>
5. Review what you pinned:  ORCA diff-note list --json
```

Notes anchor to the worktree, not to a review session — they persist in Orca's worktree
metadata and stay attached to the line until you `rm` them.

## Commands

```bash
ORCA diff-note create <path> --line <n> --body "<summary>" [--rationale "<why>"] [--author <name>] [--worktree <selector>] --json
ORCA diff-note list [--path <file>] [--worktree <selector>] --json
ORCA diff-note rm --id <noteId> [--worktree <selector>] --json
```

- `<path>` is the worktree-relative file path (first positional, or `--path`).
- `--line` is the 1-based line number on the **modified (right-hand) side** of the diff.
- `--body` is the one-line summary; `--rationale` is the optional longer explanation, rendered
  in a muted block under the summary.
- `--author` labels the note header (e.g. agent/model name); default is just "Agent".
- `--scope unstaged|staged|branch` pins the note to a specific diff view (default unstaged).
- `--worktree` defaults to the current Orca-managed worktree inferred from cwd. Pass
  `path:<abs-worktree-path>` or `id:<repoId>::<path>` when cwd is not the worktree.
- `rm` uses the note id from `diff-note list --json` (`result.comments[].id`).

## Finding the line number

`--line` is the modified-side line number in the file as it exists now, 1-based. If you
replaced line 42, anchor to 42. For a multi-line insertion prefer the first new line of the
hunk. Anchor only to lines that actually changed — Orca renders the note beside that line.

## Guiding a review

Your role is to narrate what changed and why, leaving the rationale beside the code it
explains, not in a wall of prose.

- Work in the order that tells the clearest story, not necessarily file order.
- Keep notes focused: intent, structure, risks, or follow-ups.
- Use `--body` for the decision, `--rationale` for the "why it's safe / what it depends on".
- Don't comment every hunk — pin what the user wouldn't spot themselves.
- Batch: finish editing, run `git diff`, then emit all notes in one pass rather than
  interleaving edit and note.

## Common errors

- "No Orca-managed worktree contains the current directory" — pass `--worktree path:<abs-worktree-path>`.
- "Missing required --line" — every note needs a 1-based modified-side line number.
- "selector_not_found" — the worktree isn't registered in the running Orca; confirm with `ORCA worktree ps --json`.
