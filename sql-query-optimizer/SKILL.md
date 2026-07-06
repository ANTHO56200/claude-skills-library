---
name: sql-query-optimizer
description: Diagnose and speed up a slow SQL query with EXPLAIN-driven analysis, not guesswork. Use when a query is slow, a page is slow because of the DB, or the user asks "optimize this query", "why is this slow", "add the right index". Postgres-first.
---

# SQL Query Optimizer

Optimize from evidence (the plan), not folklore. The database tells you exactly what it's
doing — read it before changing anything.

## Step 1 — Get the plan

`EXPLAIN (ANALYZE, BUFFERS) <query>` on realistic data. Read for:
- **Seq Scan** on a large table where a filter/join should use an index.
- **Rows estimated vs actual** wildly off → stale stats (`ANALYZE`) or bad predicate.
- **Nested Loop** over many rows (should be hash/merge join), or a **Sort**/**Hash** spilling to disk.
- The single most expensive node (highest actual time × loops) — fix that first.

## Common wins (apply the one the plan points to)

- **Missing index**: on the columns in `WHERE` / `JOIN` / `ORDER BY`. Composite index
  column order = equality columns first, then range/sort. Build `CONCURRENTLY` in prod.
- **Non-sargable predicate**: `WHERE fn(col) = x` or `col::text = ...` can't use an index →
  rewrite so the column is bare, or add an expression index.
- **SELECT ***: fetch only needed columns (enables index-only scans, less I/O).
- **N+1**: many small queries in a loop → one join or `WHERE id = ANY(...)`.
- **OFFSET pagination** deep in a table → keyset pagination (`WHERE id > last`).
- **Over-fetching then filtering in app** → push the filter/limit into SQL.

## Verify

Re-run EXPLAIN ANALYZE after the change. Confirm the expensive node changed (Seq Scan →
Index Scan, lower actual time). If it didn't improve, revert — an unused index is pure cost
(write amplification, planning). Measure, don't assume.

## Output

```
QUERY OPTIMIZATION
Diagnosis: <the expensive node + why, from the plan>
Change: <index / rewrite / pagination — the specific one>
Before → after: <actual ms, or estimated if no prod data>
Trade-off: <index write cost / storage — name it>
```

## Rules

- No plan, no diagnosis. Never "add an index" without the EXPLAIN that justifies it.
- Every index has a cost on writes — recommend only ones the plan will actually use.
- Realistic data volume matters: a Seq Scan on 40 rows is correct; don't optimize it.
