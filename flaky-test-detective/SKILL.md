---
name: flaky-test-detective
description: Find and fix a flaky (intermittently failing) test — the real cause, not a retry band-aid. Use when a test passes sometimes and fails others, when CI is "randomly" red, or when asked "why is this test flaky", "fix the flaky test", "stop the intermittent failures".
---

# Flaky Test Detective

A flaky test is a real bug — in the test, the code, or the environment. Retrying it just
hides the signal. Find the source of non-determinism and remove it.

## The usual causes (check in this order)

1. **Timing / async**: a hardcoded `sleep`, or asserting before a promise/callback settles.
   → wait for the condition, not a fixed delay. `await` the actual thing.
2. **Order dependence / shared state**: test A leaves state (DB row, global, module cache,
   temp file) that test B relies on or is broken by. → each test sets up and tears down its
   own state; run tests in random order to expose it.
3. **Time & randomness**: `Date.now()`, `Math.random()`, timezones, DST, locale. → inject/
   freeze the clock, seed the RNG, pin the locale/timezone.
4. **Concurrency / races**: parallel tests hitting the same resource; unordered async results
   asserted as ordered. → isolate resources per test; sort or await deterministically.
5. **External deps**: real network, real time-of-day, real filesystem. → mock the boundary;
   a test that needs the internet is an integration test, label it and isolate it.
6. **Floating point / hashing**: exact-equality on floats, or asserting an unordered map's order.

## Prove it before you fix it

Reproduce the flake: run the test in a loop (`--repeat`/`for i in $(seq 100)`), or force the
bad ordering, or freeze the timing window. A flake you can't reproduce, you haven't fixed —
you've just gotten lucky.

## Fix, don't mask

- Remove the non-determinism at the source.
- **Never "fix" a flake with a retry, a longer sleep, or `test.skip`.** That's turning off the
  smoke detector. If the flake reflects a real race in the *code*, the code has the bug — fix that.

## Output

Root cause (which category), the reproduction you used, the fix, and confirmation it now
passes N times in a row.
