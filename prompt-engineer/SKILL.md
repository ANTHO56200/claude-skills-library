---
name: prompt-engineer
description: Write or improve a prompt for an LLM so it produces reliable, correct outputs. Use when building an LLM feature, when a prompt gives inconsistent/wrong results, or asked "write a prompt for X", "improve this prompt", "why does the model do Y". Structure, examples, and guardrails over magic words.
---

# Prompt Engineer

Reliable LLM output comes from clear structure and constraints, not incantations. Tell the
model exactly what you want, show it, and box in the failure modes.

## Structure a prompt

- **Role/context**: who the model is acting as and the situation — enough to set behavior, no filler.
- **Task**: the ONE thing to do, stated imperatively and specifically. Ambiguity in → variance out.
- **Input**: clearly delimited (tags/quotes) so the model knows data from instructions.
- **Output contract**: exact format (JSON schema, structure, length). If you need machine-parseable
  output, specify the schema and say "output only that". Vague format = unparseable variance.
- **Constraints**: what to do on edge cases (empty input, uncertainty, missing data) — or the
  model will improvise inconsistently.

## Show, don't just tell (few-shot)

- 1-3 examples of input → desired output do more than paragraphs of description, especially for
  format and tone. Choose examples that cover the tricky cases, not just the easy one.
- Keep examples consistent with the rules; a contradicting example overrides your instructions.

## Reliability techniques (use what the task needs)

- For reasoning tasks: ask for the reasoning/steps before the answer (it improves correctness).
  For latency/cost or when only the answer matters, keep it terse.
- For classification/extraction: constrain the output space (enum, schema), give the "none/unknown" option.
- **Handle uncertainty explicitly**: tell it to say "I don't know" / return null rather than
  guess — otherwise it fills gaps confidently (the #1 source of "hallucination" in features).
- Put the most important instruction where it won't get lost (start and/or end).

## Iterate on evidence

- Test on real, varied inputs including the edge/adversarial ones — not one happy example.
- When it fails, diagnose WHY (ambiguous task? missing constraint? bad example?) and fix that
  specific thing. Don't randomly reword and hope.

## Rules

- Specificity and structure beat clever phrasing. There are no magic words; there are clear contracts.
- Show examples for format/tone; they're the highest-leverage part of most prompts.
- Give the model an explicit out for uncertainty, or it will guess.
- Test on varied inputs; a prompt that works on one example proves nothing.

## Output

The prompt (structured: role/task/input/output-contract/constraints + examples), a note on the
techniques used and why, and suggested test inputs including edge cases.
