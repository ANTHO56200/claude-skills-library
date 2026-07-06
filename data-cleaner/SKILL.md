---
name: data-cleaner
description: Clean and validate a messy dataset — types, missing values, duplicates, outliers, inconsistencies — without silently corrupting it. Use when prepping data for analysis/import, on a "the data is dirty" task, or asked "clean this dataset", "validate this data", "fix these records". Every transform is explicit and reversible.
---

# Data Cleaner

Dirty data produces confident wrong answers. Clean it deliberately — every change logged,
nothing dropped silently, the original kept. A "cleaned" dataset you can't trust is worse than a dirty one you can.

## Profile first (understand before changing)

- Shape: rows, columns, types (what each column ACTUALLY is vs what it should be).
- Per column: % missing, unique count, min/max/distribution, sample of odd values.
- The point: know what "dirty" means HERE before you touch anything.

## Clean deliberately (log every decision)

- **Types**: coerce to the right type; a "number" column with `"1,234"` or `"N/A"` needs a
  rule, not silent `NaN`. Decide and record the rule.
- **Missing values**: decide per column — drop the row, impute (mean/median/mode/forward-fill),
  or flag. Never impute silently on data that will be analyzed; the choice changes conclusions.
- **Duplicates**: define what "duplicate" means (exact row? same key?) before dropping. Log how many.
- **Outliers**: investigate before removing — an outlier can be the most important row (fraud,
  the bug, the whale customer) or a data-entry error. Don't auto-delete; flag and decide.
- **Inconsistencies**: normalize categories ("USA"/"U.S."/"United States"), trim whitespace,
  fix encodings, standardize dates/units. Keep a mapping of what merged into what.

## Safeguards

- Keep the raw data; write cleaned output separately. Cleaning is never in-place on the source.
- Log every transform (what, how many rows affected, the rule). The cleaning IS documentation.
- Row count in vs out reconciles — if you lost 3,000 rows, that's a decision, not an accident.

## Rules

- Never drop or impute silently — every removed/changed value is a logged, defensible decision.
- Investigate outliers before deleting; the anomaly is sometimes the signal.
- Keep the original; a reproducible cleaning script beats a hand-edited file no one can re-derive.

## Output

The cleaning script/steps (reproducible), a log of each transform + rows affected, the row
count reconciliation, and flags for anything that needs a human call (ambiguous outliers, high-missing columns).
