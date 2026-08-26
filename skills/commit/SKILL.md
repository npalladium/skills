---
name: commit
description: Stage, message, and commit/push changes safely. Use whenever creating a git commit, pushing, or force-pushing — covers staging discipline, the commit-message format, pre-commit hooks, and test gating.
metadata:
  author: npalladium
  version: "1.0.0"
---

# Committing

## What to commit
Atomic, self-contained, single-responsibility: one logical change per commit. Don't mix a refactor with a feature, or a fix with reformatting — if the subject needs an "and", it's probably two commits.
- Each commit must build and pass tests **on its own**, so `git bisect` always lands on a meaningful commit and never a broken WIP. Don't defer a fix to "the next commit".
- Split unrelated changes into separate commits; split a large change into a sequence of small, individually-tested ones.
- Once a logical change is verified and green, commit it atomically without waiting for another prompt.

## Before staging
1. `git status --short` to inventory the tree.
2. Run the repo's pre-commit hooks manually (lefthook / pre-commit / husky); fix failures before committing.
3. After multi-file changes, run the full test suite. Only commit when green.

## Staging
- Stage only files you edited this session, by explicit path: `git add path/a path/b`.
- NEVER `git add -A` or `git add .`.
- If something you didn't touch is already staged, surface it and ask before proceeding.
- For a partial commit, verify that omitted changes are not required for the staged code to build or pass applicable checks. Unrelated scratch files (notes, data, scripts, etc.) may remain untouched; do not inspect or stage them merely because they exist.

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
- After each green atomic commit, push `HEAD` to the current branch's configured upstream. If no upstream exists, surface the intended remote and branch instead of guessing.
- If an ordinary push is rejected, inspect the divergence; never force-push merely as a fallback.
- Before a confirmed force-push, record the branch, local HEAD, and remote tip; run `git pull --rebase --autostash`, recheck the rewritten range and applicable checks, then push with `--force-with-lease`. If the lease fails, stop and reassess—never retry with `--force`.

## Amending / rewriting history
- Immediately before an amend or rebase, run `git log -1` and verify that HEAD and the intended range have not moved.
- Amend automatically only when HEAD is the exact commit created for the current logical change and no later commit exists. Otherwise create a new atomic commit unless a specific rewrite was requested.
- For pushed rewrites, follow the force-push procedure above.

## Partial commits & history editing
Use interactive pickers or editors when the harness can drive and verify them reliably; otherwise use deterministic equivalents:

**Hunk-level staging:** build a patch and apply it to the index.
- `git diff -- <path> > /tmp/p.diff`, trim to the hunks you want, then `git apply --cached /tmp/p.diff` and commit (`--reverse` unstages). File-level is just `git add <path>`.

**Rewriting history — prefer primitives:**
- Squash last N: `git reset --soft HEAD~N && git commit`.
- Reword/extend HEAD: `git commit --amend`.
- Fixup an older commit: `git commit --fixup=<sha>`, then `GIT_SEQUENCE_EDITOR=true git rebase --autosquash -i <sha>~1`.

**When you need the todo list, script the editors:**
- `GIT_SEQUENCE_EDITOR='sed -i …'` rewrites the pick-list (squash/fixup/drop/reorder); `GIT_EDITOR=cat`/`true` supplies or accepts messages; `git rebase --exec '<cmd>'` runs tests per commit.
