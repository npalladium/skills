---
name: commit
description: Stage, message, and commit/push changes safely. Use whenever creating a git commit, pushing, or force-pushing — covers staging discipline, the commit-message format, pre-commit hooks, and test gating.
metadata:
  author: npalladium
  version: "1.0.0"
---

# Committing

## Before staging
1. `git status --short` to inventory the tree.
2. Run the repo's pre-commit hooks manually (lefthook / pre-commit / husky); fix failures before committing.
3. After multi-file changes, run the full test suite. Only commit when green.

## Staging
- Stage only files you edited this session, by explicit path: `git add path/a path/b`.
- NEVER `git add -A` or `git add .`.
- If something you didn't touch is already staged, surface it and ask before proceeding.

## Message format
Subject: imperative, ≤50 chars, no trailing period. Lead with one verb:

| Verb | Use for |
|------|---------|
| Add / Drop | create / delete a capability (feature, test, dependency) |
| Fix | bug, typo, accident, misstatement |
| Bump | increase a version (e.g. a dependency) |
| Make | build process, tooling, infrastructure |
| Start / Stop | enable / disable a toggle or feature flag |
| Optimize | performance only |
| Document / Rephrase | docs/comments only (Document = help files; Rephrase = textual edit) |
| Refactor / Reformat | behavior-preserving: code structure / whitespace only |

Body: wrap at 72 chars. Fields, in order — include only non-empty ones; detail
proportional to the patch's size/importance:

- `Why:` goals, use cases, the reason this change exists.
- `Changes:` how it's done — implementation, algorithm, approach.
- `Tags:` searchable keywords (optional).
- `Fixes:` / `See also:` ticket or resource links (optional).
- `Test Plan:` how it was verified (optional).

Omit a field's line entirely when empty — don't emit bare `Tags:` headers.

### Example
```
Fix race in worktree cleanup on concurrent commits

Why:
Two commits in adjacent worktrees raced on the shared lock file,
occasionally leaving a stale .git/index.lock and aborting the second.

Changes:
Take a per-worktree flock before touching the index; release in a trap
so it clears on any exit path.

Test Plan:
Ran 50x parallel commit loop across two worktrees; no stale locks.
```

## Pushing
- `git pull --rebase --autostash` before any force push.
- Force-pushing and other hard-to-reverse / outward-facing actions: confirm first unless durably authorized.

## Amending / rewriting history
- Before `git commit --amend` or a rebase, run `git log -1` and confirm HEAD is the commit you mean to rewrite. HEAD can move underneath you — new commits may have landed since yours (e.g. the user committed meanwhile) — so a blind `--amend` folds your staged change into the wrong commit and rewrites it.
- Only amend or force-push commits you authored and haven't shared.
