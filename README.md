<div align="center">

# 📚 Claude Skills Library

**45 free, MIT-licensed, token-lean skills for Claude Code.**

Security · testing · dev-workflow · AI/LLM · research · devops · frontend · 12 more categories — each skill installs in 10 seconds and teaches Claude a complete expert workflow.

![License](https://img.shields.io/badge/license-MIT-blue) ![Skills](https://img.shields.io/badge/skills-45-2dd4bf) ![Works with](https://img.shields.io/badge/works%20with-Claude%20Code-8b5cf6) ![CI](https://github.com/ANTHO56200/claude-skills-library/actions/workflows/validate.yml/badge.svg)

[Browse the skills](#the-skills) · [Install](#install) · [What makes them different](#what-makes-these-skills-different) · [Contributing](#contributing)

</div>

---

## What is this?

Agent Skills are markdown files that teach Claude Code a repeatable expert workflow — when to trigger, what process to follow, which traps to avoid. Claude loads them automatically when the task matches.

This library is a collection of skills we built for our own production work and released free: each one is **a single `SKILL.md`**, written to be token-lean (your context is a budget), with carefully engineered trigger descriptions so it fires exactly when it should.

## Install

**One skill — 10 seconds.** Pick a slug from the table below, then:

```bash
mkdir -p ~/.claude/skills/<slug> && curl -fsSL https://raw.githubusercontent.com/ANTHO56200/claude-skills-library/main/<slug>/SKILL.md -o ~/.claude/skills/<slug>/SKILL.md
```

Example — install `prompt-engineer`:

```bash
mkdir -p ~/.claude/skills/prompt-engineer && curl -fsSL https://raw.githubusercontent.com/ANTHO56200/claude-skills-library/main/prompt-engineer/SKILL.md -o ~/.claude/skills/prompt-engineer/SKILL.md
```

**The whole library:**

```bash
git clone https://github.com/ANTHO56200/claude-skills-library.git
cp -r claude-skills-library/*/ ~/.claude/skills/
```

`~/.claude/skills/` makes a skill available in every project; use `<project>/.claude/skills/` instead to scope it to one project. Restart your Claude Code session and the skills are live — no other setup.

A machine-readable index of the library ships as [`skills.json`](./skills.json).

## The skills

<!-- SKILLS:START -->

### ai-llm  <sub>(3)</sub>

| Skill | What it does |
|---|---|
| [**LLM Eval Designer**](llm-eval-designer/SKILL.md) | Turn 'it feels better' into a number — real varied cases, programmatic-first judging, per-dimension baseline, regression suite for a non-deterministic system. |
| [**Prompt Engineer**](prompt-engineer/SKILL.md) | Reliable LLM output from structure + examples + guardrails, not magic words — with an explicit out for uncertainty. Iterate on evidence. |
| [**RAG Designer**](rag-designer/SKILL.md) | End-to-end RAG that returns grounded answers — semantic chunking, hybrid retrieval + re-rank, grounded generation, separate eval. Retrieval-first. A comprehensive skill. |

### api-design  <sub>(2)</sub>

| Skill | What it does |
|---|---|
| [**OpenAPI Writer**](openapi-writer/SKILL.md) | A complete OpenAPI spec grounded in the real endpoints — every response status, reusable schemas, real examples. Not a half-spec. |
| [**REST API Designer**](rest-api-designer/SKILL.md) | Consistent, predictable REST — noun resources, right status codes, one error shape, cursor pagination, versioned. Boring beats clever. |

### code-quality  <sub>(2)</sub>

| Skill | What it does |
|---|---|
| [**Code Review**](code-reviewer/SKILL.md) | Two passes on a diff — correctness bugs first, real cleanups second, zero style nitpicks. Every blocker carries a concrete failure scenario. |
| [**Simplify Code**](simplify-code/SKILL.md) | Cut dead code, over-abstraction and needless nesting — behavior preserved, prefer deletions. Knows what NOT to touch. |

### content  <sub>(1)</sub>

| Skill | What it does |
|---|---|
| [**Newsletter Writer**](newsletter-writer/SKILL.md) | Inbox-first email that gets opened and read — one message, one CTA, 3 A/B subject variants, brand voice controls. Not SEO content. |

### data  <sub>(2)</sub>

| Skill | What it does |
|---|---|
| [**Data Cleaner**](data-cleaner/SKILL.md) | Clean a dataset without silently corrupting it — profile first, log every transform, keep the original, reconcile row counts. |
| [**Notebook Structurer**](notebook-structurer/SKILL.md) | Make an analysis notebook reproducible — Restart & Run All clean, narrative order, no hidden state, dead cells gone. |

### database  <sub>(2)</sub>

| Skill | What it does |
|---|---|
| [**DB Migration Auditor**](db-migration-auditor/SKILL.md) | Catch the outage on paper: data loss, table locks, breaking renames, missing RLS — before the migration runs. Postgres/Supabase-aware. |
| [**SQL Query Optimizer**](sql-query-optimizer/SKILL.md) | EXPLAIN-driven, not folklore: read the plan, fix the expensive node, verify it moved. Recommends only indexes the planner will use. |

### devops  <sub>(3)</sub>

| Skill | What it does |
|---|---|
| [**CI/CD Pipeline Builder**](ci-pipeline-builder/SKILL.md) | Fast feedback, safe deploys — fail-fast stage order, lockfile-keyed caching, no leaked secrets, gated progressive delivery. |
| [**Dockerfile Optimizer**](dockerfile-optimizer/SKILL.md) | Smaller, faster, safer images — multi-stage, manifest-before-source caching, non-root, no secrets in layers. Verified still-runs. |
| [**Terraform Diagnostic**](iac-terraform-diagnostic/SKILL.md) | Read the plan before you apply — catch destroy/replace of stateful resources, drift, and state traps. Diagnostic-first, not doc-dumping. |

### dev-workflow  <sub>(5)</sub>

| Skill | What it does |
|---|---|
| [**Guardrails (Token-Lean)**](guardrails-token-lean/SKILL.md) | Estimate the token bill before you spend it — right-size fan-out, model tier, and reads to the task, then stop once the returns flatten. |
| [**Plan Interrogator**](plan-interrogator/SKILL.md) | Stress-test any plan in max 3 bounded rounds — exit with a build-ready spec, not an endless interview. |
| [**Refactor (Behavior-Preserving)**](refactor-behavior-preserving/SKILL.md) | Restructure code without changing behavior, proven green by tests, scoped to what you touched. No bugfixes smuggled into a 'cleanup'. |
| [**Systematic Debugging**](systematic-debugging/SKILL.md) | Reproduce → isolate → prove the cause → fix the root. Triages trivial bugs fast, goes deep only when needed. Ships drop-in instrumentation. |
| [**TDD Workflow**](tdd-workflow/SKILL.md) | Red-green-refactor with a gate that blocks code before its failing test exists — light enough for a single function. |

### documentation  <sub>(2)</sub>

| Skill | What it does |
|---|---|
| [**Changelog Writer**](changelog-writer/SKILL.md) | Turn commits/PRs into human-first release notes — breaking changes first, internal noise dropped, semver sanity-checked. |
| [**README Writer**](readme-writer/SKILL.md) | A README a stranger can follow to running in minutes — grounded in the real code, updated incrementally, zero invented commands. |

### frontend  <sub>(3)</sub>

| Skill | What it does |
|---|---|
| [**Accessibility Audit**](accessibility-audit/SKILL.md) | WCAG audit that maps each barrier to a criterion and a concrete fix — keyboard, names, contrast, semantics, live regions. |
| [**Component Architecture**](component-architecture/SKILL.md) | Right-size a component — one responsibility, least state, composition over 15 boolean props. Fix the boundary, not the symptom. |
| [**Responsive Layout Check**](responsive-layout-check/SKILL.md) | Catch the breakpoint failures before they ship — overflow, touch targets, media, layout shift — fixed at the layout level. |

### git  <sub>(1)</sub>

| Skill | What it does |
|---|---|
| [**Git Flow**](git-flow/SKILL.md) | Conventional commits, PR descriptions and worktrees — with a validation gate that runs before you write history. |

### learning  <sub>(2)</sub>

| Skill | What it does |
|---|---|
| [**Concept Explainer**](concept-explainer/SKILL.md) | Clear AND accurate — builds from what you know, one concrete anchor (and where it breaks), progressive depth, flags every simplification. |
| [**Explain Code**](explain-code/SKILL.md) | Purpose → flow → gotchas at the altitude you asked for — reads the real code, corrects misleading names, skips line-by-line noise. |

### meta  <sub>(2)</sub>

| Skill | What it does |
|---|---|
| [**MCP Builder**](mcp-builder/SKILL.md) | Scaffold an MCP server with the patterns generators miss — auth, pagination, real errors, least-privilege — and ship it with a test. |
| [**Skill Creator (Secure)**](skill-creator-secure/SKILL.md) | Write a focused, token-lean skill AND lint it for safety before shipping — the generator that doesn't produce vulnerable skills. |

### performance  <sub>(2)</sub>

| Skill | What it does |
|---|---|
| [**Backend Profiler**](backend-profiler/SKILL.md) | Find the one dominant cost — N+1, blocking I/O, hot path — with evidence, fix by leverage, verify under real load. |
| [**Frontend Perf Auditor**](frontend-perf-auditor/SKILL.md) | Measure the waterfall/bundle/profile, fix the biggest bottleneck, verify it moved. No optimizing on hunches. |

### productivity  <sub>(2)</sub>

| Skill | What it does |
|---|---|
| [**Env Doctor**](env-doctor/SKILL.md) | 'Works on my machine', fixed: checks version → deps → env vars → ports → paths in order, and diffs against CI when local breaks but CI passes. |
| [**Memory / Handoff**](memory-handoff/SKILL.md) | Zero-infra file memory the next session picks up cold — persists decisions, constraints and gotchas, not what the repo already records. |

### project-management  <sub>(2)</sub>

| Skill | What it does |
|---|---|
| [**Decision Recorder (ADR)**](decision-recorder/SKILL.md) | Preserve WHY a decision was made and which alternatives were rejected — append-only ADRs so the reasoning survives the team. |
| [**Task Estimator**](task-estimator/SKILL.md) | Decompose until pieces are real, count the invisible work, give a range, name the unknown that makes it 3x. Estimate the work, not the deadline. |

### research  <sub>(3)</sub>

| Skill | What it does |
|---|---|
| [**Competitive Analysis**](competitive-analysis/SKILL.md) | Honest competitor teardown — includes where you LOSE, grounded in what rivals actually do, ends on the positioning gap. |
| [**Deep Research**](deep-research/SKILL.md) | Scope → fan out → adversarially verify every load-bearing claim → cited synthesis with confidence and dissent. A comprehensive research harness. |
| [**Source Summarizer**](source-summarizer/SKILL.md) | A summary you can trust instead of the original — faithful to the real claims and caveats, adds nothing, flattens nothing. |

### security  <sub>(3)</sub>

| Skill | What it does |
|---|---|
| [**Secrets Guardian**](secrets-guardian/SKILL.md) | Stop API keys, tokens and private keys from entering code, commits or logs — detect, triage false positives, and fix the pattern. |
| [**Security Review (Code)**](security-review-code/SKILL.md) | Vulnerability review of a diff with executable sweeps + OWASP/CWE mapping. Findings ranked by demonstrated exploit, each with a concrete fix. |
| [**Skill Security Auditor**](skill-security-auditor/SKILL.md) | Audit any third-party skill before you install it — prompt injection, exfiltration, dangerous commands. The check the marketplaces don't do. |

### testing  <sub>(3)</sub>

| Skill | What it does |
|---|---|
| [**E2E Test Writer**](e2e-test-writer/SKILL.md) | End-to-end tests for the flows that matter — stable selectors, self-contained data, user-visible assertions. |
| [**Flaky Test Detective**](flaky-test-detective/SKILL.md) | Kill intermittent test failures at the source — timing, shared state, clock/RNG, races — never with a retry band-aid. |
| [**Test Generator**](test-generator/SKILL.md) | Tests that would actually catch a bug — derived from the contract, one behavior each, no coverage-gaming. |
<!-- SKILLS:END -->

## What makes these skills different

- **Token-lean by design.** Your context window is a budget. Every skill is written to spend as few tokens as possible while staying unambiguous — no padding, no lore, no repeated boilerplate.
- **Clean-room content.** Written 100% from scratch for this library — not scraped, not copied, not paraphrased from other collections.
- **Trigger-engineered descriptions.** The `description:` field is the skill's activation system. Each one is written so the skill fires on the right requests — and stays silent on the wrong ones.
- **One job per skill.** A skill that does everything triggers on everything. Each of these does one thing, with a clear workflow and explicit anti-patterns.
- **CI-validated.** Every PR runs a structure check on all frontmatter (see [`validate.yml`](.github/workflows/validate.yml)).

## Contributing

Issues and PRs welcome — fixes, improvements, and new skills alike. Keep contributions in the spirit of the library: one job per skill, token-lean, valid frontmatter (`name:` matching the folder, a trigger-engineered `description:`). The CI checks structure automatically.

If a skill saved you time, a ⭐ helps other developers find the library.

## The companion system

Skills teach Claude *how to work*. **[SYNAPSE](https://github.com/ANTHO56200/synapse-memory)** teaches it *to remember* — a perpetual two-level memory (per-project + global brain shared across all projects and agents) that ships as a single markdown file. The two are built to work together.

## License & author

MIT © 2026 [Anthony Martinez](https://github.com/ANTHO56200) — founder, [The Planet Tools](https://theplanettools.ai).

*Part of the [theplanettools.ai](https://theplanettools.ai) toolbox — every skill in this library also has a detail page at [theplanettools.ai/skills](https://theplanettools.ai/skills).*
