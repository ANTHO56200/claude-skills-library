---
name: ci-pipeline-builder
description: Build or fix a CI/CD pipeline — stages, caching, secrets, fail-fast, deploy gates. Use when setting up CI, when the pipeline is slow/flaky, or asked "set up CI", "add a GitHub Actions workflow", "why is my pipeline slow", "add a deploy step". Fast feedback, safe deploys.
---

# CI/CD Pipeline Builder

CI exists to give fast, trustworthy feedback and to make deploys boring. Order it to fail
fast, cache what's expensive, and never leak a secret.

## Stage order (fail fast — cheapest checks first)

1. **Lint + typecheck** (seconds) — fail here before spending on tests.
2. **Unit tests** (fast) — the bulk of coverage.
3. **Build** — produce the artifact once, reuse it downstream.
4. **Integration / e2e** (slow) — only after the cheap stuff passes.
5. **Deploy** — gated (see below).

Run independent stages in parallel; make expensive stages depend on cheap ones passing.
A 25-minute pipeline that could be 6 is a tax everyone pays on every push.

## Caching (the biggest speed win)

- Cache dependencies keyed on the lockfile hash (restore on match, rebuild on change).
- Cache build outputs where the toolchain supports it.
- Don't cache things that must be fresh (the artifact you're testing).

## Secrets & security

- Secrets from the CI secret store / OIDC, **never** hardcoded in the workflow or echoed to logs.
- Least-privilege tokens; scope deploy credentials to what they need.
- Pin third-party actions/images by SHA, not a floating tag (supply-chain).
- Don't run untrusted PR code with access to secrets (fork PRs = no secrets by default).

## Deploy gates

- Deploy only from the main branch / tags, only after all checks pass.
- Prefer progressive delivery (staging → prod, or canary) with a health check and an
  automated rollback path. A deploy you can't roll back is a bet, not a release.

## Rules

- Cheapest checks first, expensive last — fail fast to save time and money.
- A secret in a workflow file or a log line is compromised; treat it as such.
- Make the pipeline fast enough that people don't route around it. Slow CI gets bypassed.

## Output

The pipeline config (in the project's CI system — GitHub Actions/GitLab/etc.), stage order,
caching keys, secret handling, and the deploy gate. Note what you parallelized and why.
