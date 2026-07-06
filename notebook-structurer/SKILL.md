---
name: notebook-structurer
description: Turn a messy analysis notebook into a clean, reproducible one — clear flow, no hidden state, restart-and-run-all safe. Use when a Jupyter/analysis notebook is a mess, before sharing/handing off analysis, or asked "clean up this notebook", "make this reproducible", "structure my analysis". Kills the out-of-order-execution trap.
---

# Notebook Structurer

A notebook that only runs if you execute cells in the exact order you happened to click is a
trap — for the next person and for future-you. Make it run top-to-bottom, clean, every time.

## The cardinal rule: Restart & Run All must work

Hidden state (a variable defined in a cell you later deleted, cells run out of order) makes
notebooks lie. The test of a good notebook: **Kernel → Restart & Run All** produces the same
result with no errors. If it doesn't, the notebook isn't reproducible — fix that first.

## Structure (narrative order)

1. **Title + purpose**: what question this answers, in a markdown cell up top.
2. **Imports + config**: all together, first. No imports scattered mid-notebook.
3. **Load data**: from a stable source (path/URL), with a note on what it is.
4. **Clean/prep**: transforms, each explained. (Heavy cleaning → a script, imported.)
5. **Analysis**: one logical step per section, markdown explaining WHAT and WHY before the code.
6. **Results/conclusion**: the answer, restated plainly, with the key figures.

## Clean up

- **Remove dead cells**: experiments, debugging prints, commented-out attempts. The notebook
  tells the final story, not the whole struggle.
- **No out-of-order dependencies**: each cell uses only what earlier cells defined.
- **Extract reusable logic** into functions/a module; a notebook full of copy-pasted blocks
  should be a few functions.
- **Seed randomness**, pin data versions — same input, same output.
- Clear giant outputs before committing; a notebook with 50 MB of embedded output is unshareable.

## Rules

- Restart & Run All is the acceptance test — if it errors or changes results, it's not done.
- Markdown explains intent before each step; a notebook is a document, not just code.
- Move heavy/reusable code out to a module and import it; keep the notebook as narrative.

## Output

The restructured notebook (narrative order, markdown-documented, dead cells removed), confirmation
that Restart & Run All is clean, and any logic extracted to functions/modules.
