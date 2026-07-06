---
name: git-flow
description: Write conventional commit messages, PR descriptions, and manage git worktrees with a validation gate. Use when committing changes, opening or describing a pull request, splitting mixed changes, or working multiple tasks in parallel ("commit this", "write the PR description", "set up a worktree").
---

# Git Flow

One skill for the three git moments that go wrong: the commit message, the PR
description, and parallel work. Everything here is gated — validate before you
write history, because history is forever.

## Commit messages (Conventional Commits)

Format: `type(scope): subject`

- **Types**: feat, fix, docs, style, refactor, perf, test, build, ci, chore, revert.
- **Subject**: imperative mood ("add", not "added"/"adds"), no trailing period,
  ≤ 72 chars. The subject answers "applying this commit will…".
- **Scope**: the module/area actually touched (optional but preferred).
- **Body** (only when the diff does not explain itself): the WHY, wrapped at 72.
  Never narrate the diff line by line.
- **Breaking change**: `type(scope)!: subject` AND a `BREAKING CHANGE:` footer
  describing the migration path.

### Validation gate — run BEFORE committing

1. Message matches:
   `^(feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert)(\([a-z0-9./-]+\))?!?: [a-z].{0,70}[^.]$`
2. **Diff–message match**: read the staged diff. If the diff contains changes the
   subject does not cover, either split the commit or widen the message. One
   commit = one logical change.
3. **Secret sweep** on the staged diff — block the commit on any hit of:
   `(api[_-]?key|secret|token|password|BEGIN [A-Z]+ PRIVATE KEY|sk-[A-Za-z0-9]{20,})`
   in ADDED lines. False positive? Rename the variable or add context — never wave it through.
4. No `.env`, credentials, or generated junk staged unintentionally
   (`git status` review is part of the gate, not optional).

### Breaking-change detector (check the diff for any of these)

- Removed or renamed: exported function, public route, CLI flag, config key.
- Changed: function signature, return shape, DB schema, response format.
- Added: required env var, required migration, minimum runtime version.
If any box ticks → the commit is `!` + BREAKING CHANGE footer, no exceptions.

## PR descriptions

Template — keep the whole thing under ~300 words:

```
## What
<one paragraph: the change, user-visible first>

## Why
<the problem or requirement this solves — link the issue>

## How to verify
<numbered steps a reviewer can actually run>

## Risk & rollback
<blast radius if wrong · how to revert (usually: revert this PR)>
```

Rules: no "misc fixes" PRs — split them. Screenshots for any UI change.
If "How to verify" is hard to write, the PR is too big.

## Worktrees (parallel work without stash hell)

When: reviewing a PR while mid-feature, hotfix during a refactor, two tasks in flight.

```
git worktree add ../repo-<task> -b <branch>   # create
git worktree list                             # audit what exists
git worktree remove ../repo-<task>            # after merge — always clean up
```

Hygiene: one branch per worktree, never two worktrees on one branch, remove the
worktree when the branch merges (stale worktrees are where confusion breeds).

## Anti-patterns

- `git commit -m "fix"` — if the subject could describe any commit, it describes none.
- Amending or force-pushing shared history.
- A PR description that restates the diff instead of the intent.
- Committing through a failing hook with `--no-verify`.
