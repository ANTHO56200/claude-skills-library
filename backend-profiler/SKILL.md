---
name: backend-profiler
description: Find why a backend endpoint or job is slow — the hot path, N+1 queries, blocking I/O — with evidence. Use when an API is slow, a job takes too long, CPU/latency is high, or asked "why is this slow", "profile this endpoint", "reduce latency". Measure, find the dominant cost, fix that.
---

# Backend Profiler

Backend slowness is almost always one dominant cost, not a thousand small ones. Find it with
evidence (timing, query logs, a profiler), fix that one, verify. Don't guess.

## Locate the dominant cost

- **Time the phases**: where does the request actually spend its time? DB, an external call,
  serialization, CPU compute? Instrument the boundaries or use an APM/profiler. The 80% is usually one phase.
- **The DB is the usual suspect**:
  - **N+1**: a query inside a loop (one query per row). → one join or `WHERE id IN (...)`.
    This is the single most common backend perf bug. Look for it first.
  - Slow query → hand to the `sql-query-optimizer` skill (EXPLAIN-driven: missing index,
    seq scan, bad plan).
  - Too many round trips → batch them.
- **Blocking I/O on the hot path**: a synchronous external call, disk, or lock the request
  waits on. → make it async/parallel, move it off the request, or cache it.
- **CPU**: an accidental O(n²), unbounded data loaded into memory, re-computing per request
  what could be cached.

## Fix by leverage

- Remove the work (cache, precompute, paginate so you don't load everything).
- Parallelize independent I/O instead of awaiting serially.
- Batch N calls into one.
- Only then micro-optimize code — usually the smallest lever.

## Verify

Re-measure under realistic load/data. A fix that helps on 10 rows and dies on 10k didn't fix
it. Confirm the dominant cost actually dropped.

## Rules

- Measure before touching anything; the bottleneck is rarely where it feels like it is.
- N+1 and blocking I/O on the hot path are the first two things to check — they're that common.
- Realistic data volume matters; don't optimize a query that only runs on tiny tables.

## Output

The measured hot path (with numbers), the dominant cost, the fix by highest leverage,
before→after latency, and any trade-off (memory for cache, staleness).
