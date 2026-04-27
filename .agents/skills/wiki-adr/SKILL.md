---
name: wiki-adr
description: Use when a product, architecture, infrastructure, dependency, repo-boundary, data contract, security, or operations choice becomes a durable decision that should be recorded as an ADR in the wiki.
---

# Wiki ADR

Use this skill to create or update architecture decision records.

## Workflow

1. Read `CONCEPT.md`, `wiki/decisions/index.md`, and relevant wiki/source pages.
2. Determine whether the decision is durable enough for an ADR.
3. Create the next kebab-case ADR file in `wiki/decisions/`.
4. Include:
   - title
   - status
   - date
   - context
   - decision
   - alternatives considered
   - consequences
   - sources
   - related pages
5. Update `wiki/decisions/index.md`.
6. Link back from affected wiki pages.
7. Append `wiki/log.md`.

## Rules

- ADRs are for decisions, not vague ideas.
- If the decision is still unresolved, update `wiki/open-questions.md` instead.
- If a claim lacks support, update `wiki/claims.md`.
- Keep ADRs concise and source-linked.
