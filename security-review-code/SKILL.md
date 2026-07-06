---
name: security-review-code
description: Security review of a code change or file, with executable checks and OWASP/CWE mapping. Use before shipping, on a diff, or when the user asks to "check for vulnerabilities", "security review", "is this safe to deploy". Outputs findings ranked by severity with concrete fixes.
---

# Security Review (code)

Find real vulnerabilities in the change under review — not a lecture. Every finding
names a location, a concrete exploit, a severity, and a fix. Prefer checks you can run.

## Scope first

Review the DIFF (or named files), plus any code it directly calls. Don't audit the
whole repo unless asked. New attack surface = new inputs, new queries, new outputs,
new auth paths.

## Executable sweeps (run these, cite hits)

- **Secrets in code**: `(api[_-]?key|secret|password|token)\s*[:=]\s*['"][^'"]{8,}`,
  `sk-[A-Za-z0-9]{20,}`, `BEGIN [A-Z ]+PRIVATE KEY`.
- **Injection**: string-built SQL (`"SELECT .*" + `, f-strings in queries),
  `exec/eval/system(` on user input, shell calls with unescaped variables.
- **Dependencies**: run the ecosystem audit (`npm audit`, `pip-audit`) if lockfile touched.

## Manual review (map each to CWE)

- **Input validation** (CWE-20): every external input validated + bounded before use.
- **AuthZ** (CWE-285): does this endpoint/action check *who* is allowed, not just *authenticated*?
- **SQL/command injection** (CWE-89/78): parameterized queries only; no shell string-building.
- **XSS** (CWE-79): output encoded for its sink (HTML vs attr vs JS vs URL).
- **SSRF** (CWE-918): user-controlled URLs fetched server-side — allowlist hosts.
- **Secrets/logging** (CWE-532): no secrets in logs, errors, or client responses.
- **Crypto** (CWE-327): no MD5/SHA1 for security, no hardcoded IVs/keys, TLS verified.
- **Access control on objects** (CWE-639): can user A reach user B's record by ID?

## Output

```
SECURITY REVIEW — <target>
CRITICAL / HIGH / MEDIUM / LOW (grouped)
  [severity] <title> — <file:line>
    Exploit: <concrete: input X → outcome Y>
    CWE: <id>
    Fix: <specific change>
Clean: <areas checked and found sound>
```

## Rules

- No exploit path, no CRITICAL. Speculative severity erodes trust — rank by demonstrated impact.
- "Validate input" is not a fix. Say WHICH input, WHERE, and the exact rule.
- Don't flag framework behavior you haven't confirmed is reachable/exploitable here.
