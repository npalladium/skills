---
name: commit
description: Stage, message, and commit/push changes safely. Use whenever creating a git commit, pushing, or force-pushing — covers staging discipline, the commit-message format, pre-commit hooks, and test gating.
metadata:
  author: npalladium
  version: "1.0.0"
---

# Committing

## What to commit

One logical change per commit; don't mix refactors, features, fixes, or reformatting. If the subject needs an "and", split it.

- Each commit must build and pass tests independently; split large changes into green, bisectable steps.
- Commit each verified logical change without waiting for another prompt.

## Before staging

1. Inventory the tree with `git status --short`.
2. Run repository hooks and, after multi-file changes, the full suite. Commit only when green.

## Staging

- Stage only files edited this session, using explicit paths—never `git add -A` or `git add .`.
- Surface unrelated staged changes before proceeding.
- For partial commits, ensure omitted changes aren't needed to build or test. Leave unrelated scratch files untouched and uninspected.

## Message format
Follow any commit convention provided by repository instructions already in context. Otherwise use this convention: an imperative subject of at most 50 characters with no trailing period. Lead with one verb:

| Verb | Use for |
|------|---------|
| Add / Drop | create / delete a capability (feature, test, dependency) |
| Remove | delete a capability, file, dependency, or obsolete code |
| Fix | bug, typo, accident, misstatement |
| Bump | increase a version (e.g. a dependency) |
| Update | change existing behavior, content, or configuration when no more specific verb fits |
| Make | build process, tooling, infrastructure |
| Start / Stop | enable / disable a toggle or feature flag |
| Optimize | performance only |
| Document / Rephrase | docs/comments only (Document = help files; Rephrase = textual edit) |
| Refactor / Reformat | behavior-preserving: code structure / whitespace only |
| Rename / Move | change identity or location without changing behavior |
| Test | add or revise tests without changing production behavior |
| Revert / Restore | undo a previous change or restore removed behavior |

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

- Push each green commit to its configured upstream; if none exists, surface the intended target.
- If rejected, inspect the divergence—never force-push as a fallback.
- Before a confirmed force-push, record the branch, local HEAD, and remote tip; run `git pull --rebase --autostash`, recheck, then use `--force-with-lease`. If the lease fails, stop—never use `--force`.

## Amending / rewriting history

- Before amending or rebasing, verify that HEAD and the intended range haven't moved.
- Amend automatically only when HEAD is the unchanged commit just created; otherwise commit anew unless a rewrite was requested.
- Pushed rewrites follow the force-push procedure above.

## Partial commits & history editing

Use interactive tools when reliable; otherwise use deterministic equivalents:

- Stage selected hunks by trimming a `git diff` patch and applying it with `git apply --cached`; use `--reverse` to unstage.
- Squash the last N commits with `git reset --soft HEAD~N && git commit`.
- Reword or extend HEAD with `git commit --amend`.
- Fix up an older commit with `git commit --fixup=<sha>` followed by an autosquash rebase.
- Script rebase editors when necessary; use `git rebase --exec '<cmd>'` to run checks per commit.
