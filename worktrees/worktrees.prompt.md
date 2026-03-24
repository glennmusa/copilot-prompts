---
description: "Create, list, switch, and delete git worktrees. Use when starting a new workstream, managing parallel branches, or cleaning up finished work."
agent: "agent"
argument-hint: "action and branch name (e.g., feature/docs-pipeline from main, list, switch feature/login, delete feature/old-work)"
tools: [execute, read, edit, search]
---

Create, list, switch between, and delete git worktrees — keeping the VS Code workspace file in sync.

## Determine intent

Parse the user's input to decide which action to perform:

| User says | Action |
|---|---|
| A branch name (with optional `from <base>`) | **create** |
| "list", "show", "what worktrees do I have" | **list** |
| "switch" / "open" + a worktree or branch name | **switch** |
| "delete" / "remove" + a worktree or branch name | **delete** |

Then follow the matching section below.

---

## Action: create

1. Parse:
   - **branch name** (required)
   - **base branch** (optional, default: `main`)
   - **worktree directory name** (optional) — defaults to the branch name with `/` replaced by `-`

2. Determine the current repo root by running `git rev-parse --show-toplevel` in the terminal.

3. Create the worktree one level up from the repo root:
   ```
   git worktree add ../<worktree-dir-name> -b <branch-name> <base-branch>
   ```
   If the branch already exists, omit `-b` and just check it out:
   ```
   git worktree add ../<worktree-dir-name> <branch-name>
   ```
