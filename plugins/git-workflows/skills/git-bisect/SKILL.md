---
name: git-bisect
description: Use git bisect to find the commit that introduced a bug via binary search. Use when the user wants to find a regression, track down when something broke, or narrow down a bad commit.
---

# Git Bisect

Binary search through commit history to find exactly which commit introduced a bug or regression.

## When to Apply

- User wants to find which commit introduced a bug or broke something
- User says "something worked before, when did it break?"
- User mentions regression, bisect, or "narrow down the bad commit"

## How It Works

Git bisect performs a binary search: you mark one commit as "good" (bug not present) and one as "bad" (bug present). Git checks out the midpoint, you test, mark it good or bad, and repeat until the culprit commit is identified — typically in `log₂(N)` steps.

## Workflow

1. **Start bisect**
   ```bash
   git bisect start
   ```

2. **Mark the bad commit** (where bug exists — usually HEAD)
   ```bash
   git bisect bad
   # or: git bisect bad <hash>
   ```

3. **Mark a known good commit** (before the bug existed)
   ```bash
   git bisect good <hash-or-tag>
   ```
   If you're not sure, check `git log --oneline` for a likely candidate (e.g. a recent release tag or a commit a few weeks back).

4. **Test each checkout**
   Git will check out a midpoint commit. Run the test/command that demonstrates the bug:
   - If the bug is present: `git bisect bad`
   - If the bug is absent: `git bisect good`
   - If the commit is untestable (e.g. broken build): `git bisect skip`

   Repeat until Git prints: `<hash> is the first bad commit`.

5. **Inspect the culprit**
   ```bash
   git show <hash>
   ```

6. **End bisect** (always do this — it restores your branch)
   ```bash
   git bisect reset
   ```

## Automating with a Script

If the test can be run as a command that exits `0` for good and non-zero for bad:

```bash
git bisect start
git bisect bad
git bisect good <hash>
git bisect run <your-test-command>
```

Example: `git bisect run npm test -- --testNamePattern "the failing test"`

Git runs the command at each step and marks automatically. Finish with `git bisect reset`.

## Rules

- Always end with `git bisect reset` — leaving bisect running will confuse subsequent git operations.
- Use `git bisect skip` for commits that can't be tested (broken build, missing deps) rather than guessing good/bad.
- Suggest a plausible good commit if the user isn't sure — check tags (`git tag`) or a commit from before the issue started.
- After finding the culprit, run `git show <hash>` and summarize what changed to help the user understand the cause.
