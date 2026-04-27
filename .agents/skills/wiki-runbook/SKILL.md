---
name: wiki-runbook
description: Use when documenting operational procedures, debugging playbooks, recovery steps, deployment checks, incidents, on-call procedures, or cross-repo infrastructure workflows in the wiki.
---

# Wiki Runbook

Use this skill to turn operational knowledge into repeatable procedures.

## Workflow

1. Read `CONCEPT.md`, `wiki/engineering/runbooks.md`, `wiki/engineering/infrastructure.md`, and related system pages.
2. Gather source material or implementation details.
3. Add or update a runbook with:
   - when to use it
   - symptoms
   - likely causes
   - checks
   - recovery steps
   - verification
   - prevention
   - related pages
4. If the runbook comes from a failure, update `wiki/engineering/incidents.md`.
5. If the procedure depends on unknown environment details, add an open question.
6. Append `wiki/log.md`.

## Rules

- Do not invent commands or environment names.
- Mark environment-specific unknowns explicitly.
- Link architecture, data-flow, deploy, dependency, and incident pages when relevant.
- Prefer cross-repo runbooks here; purely repo-local procedures can stay with their owning repo.
