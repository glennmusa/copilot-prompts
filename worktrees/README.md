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

Developers — and **AI coding agents** — who work on multiple branches or tasks in parallel and want to avoid the friction of stashing, committing, or losing context when switching workstreams.

## What problem it solves

### Parallel work without conflicts

The core reason this prompt exists: **multiple Copilot agents (or developers) can work on the same repository simultaneously without stepping on each other.**

Each worktree is an isolated directory on disk with its own working tree, so agents exploring a new feature, fixing a bug, or running experiments all operate in separate folders — yet they all point at the same underlying repo and share the same history. No stashing, no context loss, no risk of one agent's in-progress changes clobbering another's.

This makes it straightforward to run several agent sessions in parallel:

- One agent refactoring an API on `feature/refactor-api`
- Another investigating a bug on `bugfix/login-timeout`
- Another exploring a speculative idea on `feature/new-idea`

…all at the same time, all in the same repo, none of them interfering.

### Visual distinction across windows

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
