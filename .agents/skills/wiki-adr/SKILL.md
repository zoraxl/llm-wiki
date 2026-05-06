---
name: wiki-adr
description: Use when a product, architecture, infrastructure, dependency, repo-boundary, or operations choice becomes a durable decision that should be recorded as an ADR — outside of the normal /planning flow. For decisions that emerge ad-hoc (e.g., a choice made during implementation or review that wasn't planned ahead). If the decision came from /planning, the ADR was already created — use this skill only to amend status or consequences post-merge.
---

# Wiki ADR

Use this skill to create or update architecture decision records for decisions captured **outside of `/planning`** — i.e., choices that emerged during implementation, review, or incident response.

If a decision was made during a `/planning` session, the ADR was already written as part of that flow. Use this skill only to:
- Amend the Decision or Consequences sections after implementation reveals nuance
- Update the Status after a PR merges (preferred path: `/wiki-sync`)
- Create an ADR for a durable decision that bypassed `/planning`

## Workflow

1. Read `repos.yaml`, `CONCEPT.md`, and `wiki/decisions/index.md`.
2. Determine whether the decision is durable enough for an ADR. If still unresolved, add it to `inbox/open-questions.md` instead. If it is a claim needing source backing, add it to `inbox/claims.md` instead.
3. Pick the ADR number = max existing in `wiki/decisions/adr-*.md` + 1.
4. Create `wiki/decisions/adr-NNN-<slug>.md`:

```markdown
# ADR NNN: <Title>

Status: <proposed | accepted | superseded>
Date: YYYY-MM-DD

## Context

<Why this decision is being made>

## Decision

<What was decided>

## Alternatives Considered

<What else was considered and why it was rejected>

## Consequences

<Expected outcomes and trade-offs>

## Sources

<Relevant docs, PRs, or issues>

## Related

- [Decision index](index.md)
```

5. Update `wiki/decisions/index.md` — add a row for the new ADR.
6. Link back from affected wiki pages.
7. Append a short entry to `wiki/logs/index.md`.

## Rules

- ADRs are for decisions, not vague ideas. If the decision is still unresolved, use `inbox/open-questions.md`.
- If a claim lacks support, use `inbox/claims.md`.
- Keep ADRs concise and source-linked.
- Do not create a duplicate ADR for a decision already captured by `/planning`.
