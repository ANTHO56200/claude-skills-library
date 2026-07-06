---
name: iac-terraform-diagnostic
description: Diagnose and safely change Terraform / infrastructure-as-code — read the plan, catch destructive changes, fix drift. Use when a terraform plan is scary, apply failed, state is drifted, or asked "review this terraform", "is this apply safe", "why does it want to destroy this". Diagnostic-first, not doc-dumping.
---

# IaC / Terraform Diagnostic

Infrastructure code applies to real, running systems. The `plan` is the truth — read it
before you touch anything. Diagnose the specific situation; don't dump the whole manual.

## Read the plan like a hawk

`terraform plan` and scan the change summary. The words that matter:
- **destroy** / **-/+ (replace)**: something will be torn down. A replace = destroy THEN
  create — downtime, new IP, lost local data, new resource ID. WHY does it want to replace?
  Usually a changed immutable attribute (name, AZ, engine) forcing recreation. This is the #1
  thing to catch before `apply`.
- **update in-place (~)**: usually safe, but check WHAT (a security group rule opening 0.0.0.0/0?).
- **create (+)**: new resources — cost + does it conflict with something existing?

## Destructive-change checklist (block on any without explicit OK)

- Replace/destroy of a **stateful** resource (database, disk, bucket with data) → data loss risk.
  Look for `prevent_destroy`, snapshots, or a migration plan first.
- Change to an immutable field forcing replacement of something in production.
- Deleting/renaming a resource that others depend on.

## Drift & state

- **Drift**: real infra changed outside Terraform (someone clicked in the console). `plan`
  shows it as a diff. Decide: import the reality, or re-apply to overwrite it. Don't blindly apply.
- **State is precious**: never hand-edit state; use `import`/`state mv`/`moved` blocks.
  A corrupted state file is a very bad day. Back it up; use remote state with locking.

## Safety before apply

- `plan` reviewed by a human for any destroy/replace.
- Target-scope risky changes; apply to staging before prod.
- Know the blast radius and the rollback (previous state / re-apply old config).

## Rules

- Never `apply` a plan you haven't read — the summary line ("2 to destroy") is the warning.
- A "replace" of a stateful resource is guilty until proven safe (snapshot? acceptable downtime?).
- Diagnose THIS plan/error; don't paste generic Terraform docs. The plan already told you the problem.

## Output

Verdict: SAFE / SAFE WITH CARE / DANGEROUS. What will be destroyed/replaced and why, the
data-loss/downtime risks, the safe path (snapshot, staging-first, targeted apply), and rollback.
