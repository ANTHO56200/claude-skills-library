---
name: env-doctor
description: Diagnose a broken local dev environment fast — "works on my machine", missing env vars, wrong versions, failed install. Use when setup fails, the app won't start locally, or the user says "it won't run", "fix my environment", "works in CI but not locally". Checks the common causes in order.
---

# Env Doctor

Most "it won't run" bugs are environment, not code. Check the usual suspects in order of
likelihood before touching a single line of application logic.

## Diagnostic order (stop at the first that explains it)

1. **Runtime version.** Compare the installed version (`node -v`, `python --version`) to
   what the project pins (`.nvmrc`, `engines`, `.python-version`, `pyproject`). Mismatch is
   the #1 cause. Fix: switch to the pinned version.
2. **Dependencies actually installed + current.** Lockfile changed since last install?
   `node_modules`/venv stale or partial? → clean reinstall from the lockfile. Don't debug
   against half-installed deps.
3. **Env vars.** Compare `.env` against `.env.example` — missing or empty required keys are
   a silent killer. List exactly which are absent. (Never print their values.)
4. **Ports / services.** Is the port already in use? Is the DB/redis/service the app needs
   actually running and reachable? `lsof -i :PORT`, ping the service.
5. **Paths / OS.** Line endings (CRLF vs LF), path separators, case-sensitivity (works on
   mac, fails on Linux CI), missing system binary.
6. **Read the actual error.** Only now — the real message often names the cause once the
   environment noise is cleared.

## Compare against CI (the source of truth)

If it works in CI but not locally, diff your local against the CI config: versions,
env vars, install command, service containers. CI is a clean, reproducible environment —
your machine has drift. The diff IS the bug.

## Output

```
ENV DIAGNOSIS
Cause: <the first check that failed>
Evidence: <version mismatch / missing VAR names / port in use / …>
Fix: <exact command or step>
Also worth checking: <secondary risks, if any>
```

## Rules

- Work top-down; don't jump to exotic causes while a version mismatch sits unchecked.
- Never print env var VALUES — names only.
- If everything environmental checks out, THEN it's a code bug — hand off to systematic-debugging.
