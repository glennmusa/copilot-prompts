# Worktrees

## What it is

A Copilot Chat prompt (`worktrees.prompt.md`) for managing git worktrees — creating, listing, switching between, and deleting them.

Invoke it in Copilot Chat with `/worktrees` followed by what you want to do:

```
/worktrees create a new branch called my-feature
/worktrees list
/worktrees switch to my-feature
/worktrees delete my-feature
```

## Who it's for

Developers who work on multiple branches or tasks in parallel and want to avoid the friction of stashing, committing, or losing context when switching workstreams.

## What problem it solves

Switching branches mid-task interrupts your flow and risks losing work-in-progress. Git worktrees let you check out multiple branches simultaneously in separate directories, so you can jump between workstreams instantly.

When you're juggling multiple VS Code windows across worktrees, it's easy to lose track of which window is which branch. The prompt solves this by **styling each worktree window with a unique title bar color** based on branch convention — so you can tell at a glance which window is your feature branch, hotfix, or main line.

| Branch pattern | Title bar color |
|---|---|
| `main` / `master` | 🟢 Green |
| `develop` / `dev` | 🔵 Blue |
| `feature/*` | 🟣 Purple |
| `hotfix/*` / `bugfix/*` | 🔴 Red |
| `release/*` | 🟠 Orange |

## What the prompt does

### Create

- Creates a new worktree in a sibling directory named after the branch
- Finds or creates a `.code-workspace` file in the parent directory and adds the new folder to it
- **Styles the worktree window** with a unique title bar color based on branch naming conventions (see table above), so every VS Code window is visually distinct at a glance
- Writes color settings to `.vscode/settings.json` inside the worktree and adds that file to `.gitignore` so the colors don't pollute the repo

### List

- Runs `git worktree list` and presents results in a readable table: **Path**, **Branch**, **Status**

### Switch

- Matches your input to an existing worktree and gives you the `code <path>` command to open it directly

### Delete

- Confirms with you before removing the worktree and deleting the branch
- Removes the folder entry from the `.code-workspace` file
- Uses safe delete (`-d`) by default; only force-deletes with explicit approval
