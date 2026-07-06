---
name: test-generator
description: Generate meaningful tests for a function or module — behavior and edge cases, not coverage-gaming. Use when code lacks tests, before a refactor, or when asked "write tests for this", "add test coverage", "test this function". Tests the contract, not the implementation.
---

# Test Generator

Write tests that would actually catch a bug, not tests that inflate a coverage number.
A test that can never fail is worse than no test — it's false confidence.

## Derive cases from the contract (not the code lines)

For the function under test, enumerate:
- **Happy path**: typical input → expected output. One clear example.
- **Boundaries**: empty, zero, one, max, off-by-one neighbors, very large.
- **Invalid input**: null/undefined, wrong type, malformed — assert it fails the RIGHT way.
- **Edge semantics**: negatives, unicode, duplicates, ordering, timezone/locale if relevant.
- **Error paths**: does it throw / return an error / retry as documented?
- **Side effects**: if it writes/calls, assert that happened (with a spy/mock), and only that.

## Rules for tests that hold their value

- **One behavior per test.** The test name states the behavior:
  `returns empty array when input is empty`. Reading the names should read like a spec.
- **Arrange-Act-Assert**, no logic in the test (no loops computing the expected value —
  hardcode it, or you're testing your test).
- **Test behavior, not implementation.** Don't assert internal calls that a refactor would
  change; assert observable outcomes. Brittle tests get deleted, then you have none.
- **No shared mutable state** between tests; each sets up its own. Order-independence is non-negotiable.
- Mock only what you must (I/O, time, network). Over-mocking tests the mocks, not the code.

## Coverage, honestly

Coverage shows what ran, not what was verified. Aim for the meaningful branches and edge
cases; don't chase 100% by testing getters. Name any behavior you deliberately left untested.

## Output

The test file (matching the project's framework + conventions — read an existing test
first), each test named for its behavior, plus a one-line note on what's covered and what isn't.
