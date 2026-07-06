---
name: db-migration-auditor
description: Audit a database migration for danger BEFORE it runs — data loss, table locks, breaking changes. Use when writing/reviewing a schema migration, ALTER, or any DDL ("check this migration", "is this safe to run in prod", "review this schema change"). Postgres/Supabase-aware.
---

# DB Migration Auditor

A migration touches a live database with real data. Treat every DDL as guilty until
proven safe. This audits the SQL before it runs — catch the outage on paper, not in prod.

## Destructive-change checks (CRITICAL — block on any)

- `DROP TABLE` / `DROP COLUMN` — permanent data loss. Confirm the data is truly dead or
  archived. Prefer a two-phase deprecation (stop writing → drop later).
- `ALTER COLUMN ... TYPE` — can rewrite the whole table + fail on incompatible data.
- `RENAME` (table/column) — breaks every reader still using the old name → coordinate with code deploy.
- `TRUNCATE`, `DELETE` without `WHERE`, `UPDATE` without `WHERE`.
- `NOT NULL` added to an existing column with no default → fails if any row is null; may lock.

## Lock / performance checks (HIGH on a large table)

- Postgres: adding a column with a **volatile default**, `ALTER TYPE`, or an index built
  WITHOUT `CONCURRENTLY` takes an ACCESS EXCLUSIVE lock → writes blocked for the whole build.
  → use `CREATE INDEX CONCURRENTLY`, add-column-then-backfill-in-batches, `NOT VALID` +
  `VALIDATE CONSTRAINT` for FKs/checks.
- Long-running backfill inside the migration transaction → holds locks; batch it outside.

## Correctness checks

- **Reversible?** Is there a matching down-migration, or is this one-way? Say so.
- **Idempotent?** `IF NOT EXISTS` / `IF EXISTS` so a re-run doesn't explode.
- **Order vs code**: does the app deploy expect this before or after? Additive-first
  (add nullable col → deploy code → backfill → enforce) avoids downtime.
- **RLS (Supabase)**: new table has RLS enabled + policies? A new table without RLS is
  world-readable via the anon key — a classic Supabase leak.

## Output

```
MIGRATION AUDIT — <file>
Verdict: SAFE | SAFE WITH CHANGES | DANGEROUS
CRITICAL: <data-loss / no-WHERE / etc.>
HIGH (locks): <blocking operations on large tables + the concurrent alternative>
Reversibility: <down-migration present? one-way?>
Deploy order: <additive-first plan if needed>
RLS: <enabled + policies? (Supabase)>
```

## Rules

- Assume the table is large and in production unless told otherwise. The safe path costs little; the outage costs a lot.
- Never approve an irreversible data-loss step without an explicit "yes, the data is gone on purpose".
- Treat SQL as code: parameterize, no string-built dynamic SQL in app-run migrations.
