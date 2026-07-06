---
name: llm-eval-designer
description: Build an evaluation set and judging method for an LLM feature so you can measure quality and catch regressions. Use before/while shipping an LLM feature, when "it feels worse", or asked "how do I test my prompt/LLM", "build an eval", "measure LLM quality". Turns vibes into a number you can trust.
---

# LLM Eval Designer

"It seems better" is not shippable. An eval turns subjective quality into a repeatable
measurement so you can compare prompts/models and catch regressions before users do.

## 1 — Define what "good" means (concretely)

- Pick the 2-4 dimensions that matter for THIS feature: correctness/accuracy, faithfulness
  (grounded, no invention), format compliance, safety, tone, latency/cost. Vague goals can't be measured.
- For each, define pass/fail or a scale with clear anchors — "helpful" is unmeasurable;
  "answers the question using only the provided context, correct format" is.

## 2 — Build the eval set

- **Real, varied inputs**: pulled from actual/expected usage, not cherry-picked easy ones.
  Include the edge cases, the adversarial inputs, and the "should refuse/say I don't know" cases.
- **Coverage over volume**: 30-50 well-chosen cases spanning the failure modes beat 500 similar
  easy ones. Each case = input + what a good output must satisfy.
- Include known-hard and previously-failed cases as regression guards.

## 3 — Choose a judging method (fit to the dimension)

- **Programmatic** where possible — the most reliable: exact/structural match, schema-valid,
  contains-required, regex, latency threshold. Cheap, deterministic. Use it for format/constraints.
- **LLM-as-judge** for subjective quality (faithfulness, helpfulness): give the judge a clear
  rubric and the criteria, ask for a score + reason. Beware its biases (length, position,
  self-preference); validate the judge against some human labels before trusting it.
- **Human** for the highest-stakes/subtle calls — on a sample, to calibrate the automated judges.

## 4 — Run & act

- Score the current version → a baseline number per dimension. Now every prompt/model change is
  measured against it, not guessed. A change that helps one case and breaks three shows up.
- Track per-dimension (an aggregate can hide a safety regression under an accuracy gain).
- Re-run on every change; the eval is your regression suite for a non-deterministic system.

## Rules

- Measure the dimensions that matter with clear pass criteria — unmeasurable goals stay vibes.
- Prefer programmatic checks; reserve LLM/human judging for what code can't score, and validate the judge.
- Cover failure modes and refusal cases, not just the happy path; that's where regressions hide.
- One number for a subjective feature is a start, not the truth — keep a human in the loop on a sample.

## Output

The eval set (cases with input + success criteria), the judging method per dimension
(programmatic/LLM/human) with rubrics, a baseline score, and how to re-run it on each change.
