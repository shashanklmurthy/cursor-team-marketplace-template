---
name: git-tag
description: Create, list, push, and delete git tags for releases using semantic versioning. Use when the user wants to tag a release, create a version tag, push tags to remote, or delete a tag.
---

# Git Tag

Create and manage version tags for releases using semantic versioning.

## When to Apply

- User wants to tag a release or create a version tag
- User asks about `git tag`, pushing tags, or deleting a tag
- User mentions semver, a release number, or "cut a release"

## Semantic Versioning Primer

Tags should follow `vMAJOR.MINOR.PATCH`:
- **MAJOR** — breaking changes
- **MINOR** — new features, backwards-compatible
- **PATCH** — bug fixes, backwards-compatible

Find the latest tag to determine what to bump next:
```bash
git tag --sort=-v:refname | head -5
```

## Commands

| Goal | Command |
|------|---------|
| List tags (sorted) | `git tag --sort=-v:refname` |
| Create annotated tag | `git tag -a v1.2.3 -m "Release v1.2.3"` |
| Tag a specific commit | `git tag -a v1.2.3 <hash> -m "Release v1.2.3"` |
| Push one tag | `git push origin v1.2.3` |
| Push all tags | `git push origin --tags` |
| Delete local tag | `git tag -d v1.2.3` |
| Delete remote tag | `git push origin --delete v1.2.3` |

## Workflow

1. **Determine the next version**
   - Run `git tag --sort=-v:refname | head -5` to see recent tags.
   - Ask or infer from context whether this is a major, minor, or patch bump.

2. **Create an annotated tag** (always prefer annotated over lightweight)
   ```bash
   git tag -a v1.2.3 -m "Release v1.2.3"
   ```
   Annotated tags store metadata (author, date, message) and are the standard for releases.

3. **Push the tag to remote**
   ```bash
   git push origin v1.2.3
   ```
   Prefer pushing a single tag over `--tags` to avoid accidentally pushing unintended tags.

4. **Confirm**
   ```bash
   git tag --sort=-v:refname | head -3
   ```

## Rules

- Always use **annotated** tags (`-a`) for releases, not lightweight tags.
- Default message format: `"Release vX.Y.Z"` unless the user specifies otherwise.
- Prefer `git push origin <tag>` over `--tags`.
- Do not delete or move a tag that has already been pushed to remote without explicit user confirmation — this can break other users' references.
- If the user didn't specify a version, suggest the next logical version based on `git tag --sort=-v:refname` output.
