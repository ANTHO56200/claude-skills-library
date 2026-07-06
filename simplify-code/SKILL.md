---
name: simplify-code
description: Reduce complexity in existing code — dead code, over-abstraction, deep nesting, needless indirection — without hunting bugs. Use when the user says "simplify", "this is over-engineered", "reduce complexity", "make this readable". Quality-only, behavior preserved.
---

# Simplify Code

Make code simpler and clearer while keeping behavior identical. This is not a bug hunt
and not a rewrite — it's removing what shouldn't be there.

## What to cut

- **Dead code**: unreachable branches, unused vars/params/imports/exports, commented-out blocks.
- **Over-abstraction**: a base class with one subclass, a factory that builds one thing,
  a config layer for a constant, indirection that adds a hop and no value. Inline it.
- **Deep nesting**: replace with guard clauses / early returns; flatten arrow-code.
- **Reinvented stdlib**: hand-rolled loops that are `map`/`filter`/`find`; manual clamping
  that's `Math.min/max`; date math that's a one-liner.
- **Boolean/flag params** that fork behavior → two clearly named functions.
- **Redundant state**: a variable derivable from another; caching that isn't needed.

## What to LEAVE

- Complexity that carries its weight (real perf, real requirement, real edge case) — a
  comment explaining WHY should make you keep it.
- Anything you can't prove is behavior-neutral. When unsure, leave it and say why.
- Clever-but-clear code. Simple ≠ dumbed-down. Don't expand a crisp expression into ten lines.

## Method

1. Confirm tests cover it (or add a quick characterization test).
2. One simplification at a time → run tests → green → next.
3. Prefer deletions over additions. The best simplification removes lines.

## Output

Report: lines removed, each simplification (before → after in one phrase), behavior
unchanged + green tests. If you found complexity worth keeping, name it and why.
