---
name: wiki-ingest
description: Use when adding source material, implementation discoveries, PR summaries, meeting notes, incident notes, or engineering changes into an LLM wiki. Updates synthesized pages, source maps, open questions, claims, engineering pages, and the wiki log while preserving raw source authority.
---

# Wiki Ingest

Use this skill to add durable knowledge to the wiki.

## Workflow

1. Read `CONCEPT.md`, `sources/index.md`, and `wiki/index.md`.
2. Read the provided source material or referenced files.
3. Classify the material:
   - product concept or terminology
   - architecture
   - infrastructure
   - environment or deploy flow
   - repo structure or ownership
   - dependency or integration decision
   - data contract or API boundary
   - runbook or incident
   - open question
   - claim needing source review
4. Update the smallest relevant wiki pages.
5. If a new page is created, update `wiki/index.md`.
6. If a new authoritative source is introduced, update `sources/index.md`.
7. Add unresolved items to `wiki/open-questions.md`.
8. Add important assertions needing review to `wiki/claims.md`.
9. Append a short entry to `wiki/log.md`.

## Rules

- Do not copy repo-local docs wholesale into the wiki.
- Preserve source authority with links or paths.
- Synthesize; do not mirror.
- Mark uncertainty explicitly.
- Do not present speculation as settled knowledge.
- If the material creates a durable architecture decision, suggest or create an ADR.
- If the material changes operations, update `wiki/engineering/runbooks.md` or `wiki/engineering/incidents.md` as appropriate.

## Output

In the final response, list pages updated, sources consulted, open questions added, claims added, and any ADR/runbook recommendation.
