---
name: refactor-behavior-preserving
description: Refactor or simplify code without changing behavior, verified by tests. Use when cleaning up, reducing duplication, improving structure, or when the user says "refactor this", "simplify", "clean this up", "improve the architecture". Scoped and safe by construction.
---

# Refactor (behavior-preserving)

Refactoring changes structure, never behavior. If behavior changes, it's not a
refactor — it's a rewrite, and it needs its own review. Stay scoped and stay green.

## Preconditions (don't start without these)

- **A safety net exists.** Tests cover the code you're about to move. If they don't,
  write characterization tests FIRST (capture current behavior, even if "wrong") — then refactor.
- **Scope is bounded.** Refactor what's in front of you (the file/function you touched),
  not the whole codebase. Sweeping refactors hide behind "cleanup" and break things far away.

## The move

1. Run tests → green baseline.
2. Make ONE structural change (extract function, rename, dedupe, invert a condition,
   replace a magic value). Small and named.
3. Run tests → still green. If red, revert THIS step (not the whole session) and rethink.
4. Repeat. Commit at green checkpoints so you can always fall back.

## High-value, low-risk targets

- Duplicated logic → one named function (only if the duplicates are truly the same intent).
- Deep nesting → early returns / guard clauses.
- Magic numbers/strings → named constants.
- Long function → extract cohesive steps with intention-revealing names.
- Unclear name → precise name (the cheapest, highest-value refactor there is).

## Guardrails

- **No behavior change smuggled in.** No "while I'm here" bugfixes or feature tweaks —
  do those as separate, reviewed changes. One kind of change per commit.
- Don't refactor for its own sake. If the code is clear and stable, leave it. Churn has
  cost (review, risk, blame history) and cosmetic churn buys nothing.
- Preserve the public interface unless the task is explicitly an interface change.

## Output

Report: what changed structurally, what stayed identical (behavior/interface), and the
green test run that proves it.
