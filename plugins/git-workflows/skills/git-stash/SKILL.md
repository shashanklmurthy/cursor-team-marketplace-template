---
name: git-stash
description: Stash working changes, switch context, and reapply with git stash. Use when the user wants to stash changes, save work in progress, switch branches without committing, or reapply stashed changes.
---

# Git Stash

Temporarily put aside uncommitted changes so you can switch branches, pull, or work on something else, then bring them back.

## When to Apply

- User asks to stash changes, save WIP, or "put this aside"
- User wants to switch branches but has uncommitted changes
- User wants to pull or rebase but has local changes
- User asks to apply, pop, or list stashes
- User mentions "stash" in the context of git

## Commands

| Goal | Command |
|------|---------|
| Stash (working dir + index) | `git stash push -m "description"` |
| Stash including untracked files | `git stash push -u -m "description"` |
| List stashes | `git stash list` |
| Apply stash (keep in list) | `git stash apply [stash@{n}]` |
| Pop stash (apply + drop) | `git stash pop [stash@{n}]` |
| Drop a stash | `git stash drop stash@{n}` |
| Show stash diff | `git stash show -p stash@{n}` |

## Workflow

1. **Stashing**
   - Prefer a short message: `git stash push -m "wip: auth form"`.
   - Use `-u` (or `--include-untracked`) if they need to stash new files too.
   - If they have nothing to stash, `git status` will show clean; suggest they may already be clean or that changes are only in ignored files.

2. **Reapplying**
   - **Pop** when they want the stash applied and removed from the list (default: most recent). If they might use the same stash again, suggest **apply** instead.
   - If pop causes conflicts, the stash is not dropped; help resolve conflicts and then they can `git stash drop` when done.

3. **Inspecting**
   - Use `git stash list` to see stash refs (e.g. `stash@{0}`).
   - Use `git stash show -p stash@{n}` to show the diff before applying or popping.

## Rules

- Default to the most recent stash (`stash@{0}`) when no index is given.
- Run commands from the repo root; ensure we're in a git repo before stashing.
- Prepend `GIT_EDITOR=true` to any git command that might open an editor (e.g. `git stash` without `-m`), to avoid blocking.
- Don't stash when the user asked to commit; suggest committing instead if the work is ready.
