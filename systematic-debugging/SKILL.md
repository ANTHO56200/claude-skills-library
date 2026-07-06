---
name: systematic-debugging
description: Root-cause a bug methodically instead of guessing. Use when something is broken, failing, flaky, or behaving unexpectedly ("debug this", "why is this failing", "it works locally but not in prod"). Triages fast, then goes deep only if needed.
---

# Systematic Debugging

Guess-and-check wastes hours. Reproduce, isolate, prove the cause, then fix. Triage
first so trivial bugs don't get the full ceremony.

## Step 0 — Triage (30 seconds)

- Read the actual error + stack trace fully. The answer is often the first line you skimmed.
- Obvious typo / missing import / null on a named line? Fix it, move on. Not every bug needs the method.
- Otherwise → go systematic below.

## The method

1. **Reproduce reliably.** Find the smallest, deterministic trigger. A bug you can't
   reproduce, you can't fix — you can only hope. If it's flaky, reproduce the flakiness
   (loop it, control the timing/ordering).
2. **Locate.** Bisect the distance between "known good" and "observed bad": comment out
   halves, add checkpoints, `git bisect` across commits. Narrow to the smallest region
   that still shows the bug.
3. **Hypothesize + instrument.** State one falsifiable cause ("the list is empty because
   the filter runs before the fetch resolves"). Add a log/breakpoint that would PROVE OR
   DISPROVE it. Run. Let evidence, not vibes, confirm.
4. **Fix the cause, not the symptom.** Patch the root, not the place it surfaced. If you
   find yourself special-casing the output, you're treating the symptom.
5. **Verify + guard.** Confirm fixed. Add a test that reproduces the original bug so it
   can't return. Remove debug instrumentation.

## Instrumentation snippets (drop-in)

- Trace value + location: `console.log("[dbg] X @ handler", { x, type: typeof x })`.
- Prove ordering: log with timestamps at each async boundary.
- Catch the swallow: temporarily log inside every `catch` — silent catches hide bugs.

## Anti-patterns (refuse)

- Changing things at random hoping one sticks. If you don't know why a change would fix
  it, you don't understand the bug yet.
- **"Fixing" the test to make it pass.** That hides the bug for the next person.
- Declaring victory on "it works now" without knowing WHY it broke — it'll be back.
