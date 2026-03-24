# copilot-prompts

A collection of prompts for GitHub Copilot in VS Code, organized by use case.

## Prompts

| Prompt | What it does |
|--------|-------------|
| [worktrees](./worktrees/) | Create, list, switch, and delete git worktrees. Built for running **multiple Copilot agent sessions in parallel** — each agent gets its own isolated directory on the same repo, so they can explore features and fix bugs without stepping on each other. |

## How to install a prompt

Copy any `.prompt.md` file into your VS Code user prompts directory:

| OS | Path |
|----|------|
| macOS / Linux | `~/.config/Code/User/prompts/` |
| Windows | `%APPDATA%\Code\User\prompts\` |

For example, to install the worktrees prompt on macOS/Linux:

```bash
cp worktrees/worktrees.prompt.md ~/.config/Code/User/prompts/
```

Once copied, the prompt will appear in Copilot Chat when you type `/` followed by the prompt file name (without the `.prompt.md` extension).

## How to use

Each prompt directory contains:

- **README.md** — what the prompt is, who it's for, and what problem it solves
- **EXAMPLES.md** — ready-to-run prompts you can paste into Copilot Chat
