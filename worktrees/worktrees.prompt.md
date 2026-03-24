
4. Look for an existing `.code-workspace` file in the parent directory of the repo root. If none exists, create one. If one exists, update it.

5. Add the new worktree folder to the workspace file's `"folders"` array with a human-readable `"name"` derived from the branch name.

6. Ensure the workspace file's `"settings"` object includes a `window.title` that shows the worktree name first:
   ```json
   "settings": {
     "window.title": "[${activeRepositoryBranchName}] ${dirty}${activeEditorShort}${separator}${rootName}${separator}${profileName}${separator}${appName}"
   }
   ```
   Only add this setting when first creating the workspace file — do not overwrite it if it already exists.

7. **Assign a unique title bar color to the new worktree folder** so the user can visually distinguish it when opened in its own VS Code window.

   > **Why per-folder, not per-workspace?** `workbench.colorCustomizations` is a window-level setting — VS Code does not support scoping it per folder inside a multi-root workspace. The colors take effect when a worktree folder is opened on its own (e.g. `code <worktree-path>`).

   7a. Determine the worktree name from the workspace root folder name.
   7b. Pick a **unique, visually distinct** background color for the title bar based on common branch conventions:
   - `main` / `master` — green (e.g. `#1e6e2e`)
   - `develop` / `dev` — blue (e.g. `#1a5276`)
   - `feature/*` — purple (e.g. `#7d3c98`)
   - `hotfix/*` / `bugfix/*` — red (e.g. `#c0392b`)
   - `release/*` — orange (e.g. `#b9770e`)
   - For anything else, choose a color that hasn't been used by the above.
   7c. Create or update `.vscode/settings.json` with these settings, **merging with any existing settings** (do not overwrite unrelated keys):

   ```json
   {
   "workbench.colorCustomizations": {
      "titleBar.activeBackground": "<chosen-color>",
      "titleBar.activeForeground": "#ffffff",
      "titleBar.inactiveBackground": "<chosen-color>99",
      "titleBar.inactiveForeground": "#ffffffcc"
   },
   "window.title": "[${rootName}] ${dirty}${activeEditorShort}${separator}${appName}"
   }
   ```

   - The inactive background must be the active color with `99` appended (60% opacity).
   - Foreground colors stay white (`#ffffff` active, `#ffffffcc` inactive).
   - Always preserve existing settings in `.vscode/settings.json` — only add or update the keys above.
   - If the user specifies a color, use that color instead of the convention above.

8. **Add `.vscode/settings.json` to the worktree's `.gitignore`** so the color customizations don't show up as tracked changes or get committed.
   - If a `.gitignore` file already exists in the worktree root, append `.vscode/settings.json` to it (only if the entry isn't already present).
   - If no `.gitignore` exists, create one containing `.vscode/settings.json`.

9. Report back:
   - The worktree path created
   - The branch name
   - The workspace file path
   - Suggest opening the new worktree folder directly in VS Code: `code <worktree-path>`, so the user lands in the exact worktree they just created.

---

## Action: list

1. Run `git worktree list` in the terminal.
2. Present the results in a readable table with columns: **Path**, **Branch**, and **Status** (bare, clean, prunable, etc.).
3. Note which worktree is the current one.

---

## Action: switch

1. Run `git worktree list` to find available worktrees.
2. Match the user's input (branch name or directory name) to a worktree path.
3. If a `.code-workspace` file exists in the parent directory, confirm it already contains the target folder.
4. Provide the command to open the target worktree folder directly in VS Code:
   ```
   code <worktree-path>
   ```

---

## Action: delete

1. Run `git worktree list` to confirm the worktree exists.
2. Match the user's input (branch name or directory name) to a worktree.
3. **Guard rails — ask the user for confirmation before proceeding**, showing:
   - The worktree path that will be removed
   - The branch that will be deleted
4. After confirmation, execute:
   ```
   git worktree remove <worktree-path>
   git branch -d <branch-name>
   ```
   - If the branch has unmerged changes, warn the user and do **not** use `-D` unless they explicitly confirm force-deletion.
5. Update the `.code-workspace` file: remove the deleted worktree's entry from the `"folders"` array.
6. Note: the `.vscode/settings.json` inside the worktree folder (containing color customizations) is automatically removed when the worktree directory is deleted.
7. Report what was removed.

---

## Constraints

- Never force-delete or reset existing worktrees without explicit user confirmation
- If a target directory already exists during create, warn the user and stop
- Always verify git operations succeeded before modifying the workspace file
- When deleting, always attempt a safe delete (`-d`) first; only escalate to force delete (`-D`) with explicit user approval
