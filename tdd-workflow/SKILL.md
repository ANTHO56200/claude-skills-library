---
name: tdd-workflow
description: Test-driven development with a real red-green-refactor gate. Use when building a new function/feature/bugfix and the user wants tests to drive the code ("do this TDD", "write the test first", "test-drive this"). Enforces test-before-code without the ceremony.
---

# TDD Workflow

Red → Green → Refactor, with a gate that actually blocks. Lightweight enough to use on
a single function, not just a whole feature.

## The loop (one behavior at a time)

1. **RED** — write ONE failing test for the next smallest behavior. Run it. Confirm it
   fails *for the right reason* (assertion, not import error). A test that never failed
   proves nothing.
2. **GREEN** — write the minimum code to pass. Not the elegant version — the passing
   version. Run the test. It passes.
3. **REFACTOR** — now improve names/structure with the test as your safety net. Run
   again. Still green. Only refactor on green.
4. Repeat for the next behavior.

## The gate (enforce, don't trust)

Before writing any implementation line, the matching test must exist AND have been run
red. If you're about to write code with no failing test pointing at it — stop, back up,
write the test. This is the whole discipline; everything else is style.

Practical gate you can automate: block the commit if a new exported function has no
referencing test file. Rough check — new `export function foo` with zero `foo(` in any
`*.test.*`/`*.spec.*` → fail with "no test drives foo".

## What to test (and not)

- Test **behavior and contracts**: inputs → outputs, edge cases, error paths.
- Test the **bug** before fixing it: a failing test that reproduces it, then the fix.
- Don't test framework internals, getters, or the language. Don't test implementation
  details that lock in structure (test what it does, not how).

## Anti-patterns (refuse)

- Writing the code, then a test that just mirrors it (that's not TDD, it's decoration).
- **Editing the test to match a wrong implementation.** The test encodes intent; if it's
  failing, suspect the code first.
- Giant tests covering five behaviors — one behavior, one test, one reason to fail.
- Skipping RED "because it's obviously right" — obviously-right code fails tests weekly.

## When to skip TDD (be honest)

Throwaway spikes, exploratory prototypes, pure config. Say so out loud rather than
faking a test afterward. Convert the spike with tests once the design is known.
