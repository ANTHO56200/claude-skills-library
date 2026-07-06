---
name: task-estimator
description: Break down work and estimate it realistically, with the risks named. Use when planning a feature, sizing a task, giving a timeline, or asked "how long will this take", "break this down", "estimate this work". Decomposes, sizes honestly, and surfaces the unknowns that blow up estimates.
---

# Task Estimator

Estimates blow up because of the work nobody listed and the unknowns nobody flagged. Decompose
until the pieces are real, size honestly, and name what could make it 3x. An estimate without
its risks is a guess with a number.

## Decompose first (you can't estimate a blob)

- Break the work into pieces small enough that each is understood and independently sized —
  roughly half-a-day to a couple of days each. A "2 weeks" estimate on an undivided task is fiction.
- **Include the invisible work**, which is where estimates die: tests, error handling, edge
  cases, code review + rework, integration, migration/backfill, docs, deployment, and the
  "make it actually production-ready" tail. The happy-path code is often the small part.

## Size honestly

- Estimate effort per piece from similar past work if you have it (reference beats intuition).
- Give a range, not a point: optimistic / likely / pessimistic. A single number pretends to a
  certainty that doesn't exist. The spread communicates the uncertainty.
- **Don't anchor to the hoped-for deadline.** Estimate the work, then compare to the deadline —
  not the reverse. Wishful estimates just move the disappointment later.

## Surface the risks (the real value)

- **Unknowns**: the pieces you don't understand yet — a new API, an unclear requirement, a
  dependency on someone else. These have the widest spread; flag them as the top risk and
  consider a spike to de-risk before committing.
- **Assumptions**: state them ("assumes the API supports X", "assumes no data migration"). A
  broken assumption is where the 3x comes from.
- **Dependencies**: work blocked on other people/teams/decisions — outside your control, so outside your estimate's reliability.

## Rules

- Decompose until the pieces are real; you can't honestly size what you haven't broken down.
- Count the invisible work (tests, review, edges, deploy) — it's usually most of the total.
- Give a range and name the unknowns; estimate the work, don't reverse-engineer the deadline.
- The biggest unknown deserves a spike before a commitment, not a confident number.

## Output

The breakdown (pieces + size each), a total range (optimistic/likely/pessimistic), the top
risks and assumptions, and the one or two unknowns worth de-risking first.
