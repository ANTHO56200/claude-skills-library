---
name: e2e-test-writer
description: Write end-to-end tests for a real user flow, resilient to UI churn. Use when adding tests for a critical path (signup, checkout, core action), or when asked "write an e2e test", "test this user flow", "add a Playwright/Cypress test". Tests what the user does, stably.
---

# E2E Test Writer

Test the flows that would cost money or trust if they broke — from the user's point of
view, through the real UI. Keep them stable so they catch real regressions, not CSS tweaks.

## Pick the right flows (don't e2e everything)

E2E is slow and expensive; reserve it for critical paths: auth, checkout/payment, the core
value action, anything that would be an incident if broken. Everything else belongs in
faster unit/integration tests.

## Write for stability

- **Select by role/label/test-id**, never by CSS class or DOM position — those change with
  every restyle and make tests flaky. Prefer accessible selectors (role, label, text).
- **Wait for state, not time.** Await the element/response, never a fixed `sleep`.
- **Independent + idempotent**: each test creates its own data and cleans up; no reliance on
  a previous test or a pre-seeded account that may drift.
- **One user journey per test**, asserting the outcomes a user would notice (URL, visible
  text, a created record) — not internal state.

## Structure

```
test("<user goal>", ...):
  arrange: seed the minimal data / navigate to start
  act:     perform the real steps (fill, click, submit) as a user would
  assert:  the user-visible outcome (confirmation, redirect, new item)
  cleanup: remove created data
```

## Rules

- Test the happy path of each critical flow first; add the top 1-2 failure cases (bad
  password, declined card) second. Don't try to e2e every branch — that's unit-test work.
- If a test needs a `sleep` to pass, it's hiding a race — wait on the condition instead.
- Keep the suite fast enough that people actually run it; a 40-minute e2e suite gets skipped.

## Output

The e2e test(s) in the project's framework (read an existing one for conventions), stable
selectors, self-contained data, asserting user-visible outcomes.
