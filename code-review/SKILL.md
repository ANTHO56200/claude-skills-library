---
name: code-review
description: Review a diff for correctness bugs and cleanup opportunities, with severity and file:line anchors. Use on a pull request, a working diff, or when asked to "review this code", "check my changes". Two passes — correctness first, then reuse/simplicity — never style nitpicks.
---

# Code Review

Review the CHANGE, not the whole repo. Find bugs that will actually bite, then genuine
cleanups. Anchor every finding to a location. Skip subjective style — the linter owns that.

## Pass 1 — Correctness (the one that matters)

For each changed hunk, ask:
- **Does it do what it claims?** Off-by-one, inverted condition, wrong operator, wrong variable.
- **Edge cases**: null/undefined/empty, zero, negative, very large, unicode, concurrent access.
- **Error paths**: what happens when the call fails? Swallowed error? Partial write? No rollback?
- **Contracts**: return shape matches callers; async actually awaited; resources closed.
- **Regressions**: does this break an existing caller or assumption? Check the call sites.
- **Security**: user input reaching a query/shell/HTML sink (hand off to security-review if deep).

## Pass 2 — Reuse & simplicity (only real wins)

- Duplicate of an existing helper? Point to the helper.
- Dead code, unreachable branch, unused variable introduced by the change.
- Needless complexity: a loop that's a `map`, nesting that's an early return, a flag that's two functions.
Only raise these if they're clear improvements — don't invent work.

## Severity

- **BLOCKER**: will produce wrong results, data loss, crash, or a vuln. Must fix before merge.
- **SHOULD**: real bug in an edge case, or a meaningful cleanup. Fix or consciously accept.
- **NIT**: minor; optional. Keep these rare — a review that's 90% nits gets ignored.

## Output

```
REVIEW — <target> (<n> files)
BLOCKER / SHOULD / NIT
  [severity] <file:line> — <what's wrong> → <fix>
      Fails when: <concrete input/state → wrong outcome>   (for bugs)
Looks good: <what's solid — so the author knows it was actually read>
```

## Rules

- Every BLOCKER needs a concrete failure scenario. "This looks risky" is not a finding.
- Don't restate the diff. Don't ask for changes the diff already made. Don't nitpick formatting.
- Review the code that's there, not the code you'd have written. Different ≠ wrong.
