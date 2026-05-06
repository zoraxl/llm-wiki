---
name: wiki-query
description: Use when retrieving knowledge from the team wiki, including product concept, architecture, infrastructure, repo changes, decisions, runbooks, incidents, open questions, and source-backed claims. Answers should cite relevant wiki/source pages and distinguish stable knowledge from uncertainty.
---

# Wiki Query

Use this skill to answer questions from the wiki.

## Workflow

1. Read `CONCEPT.md`, `repos.yaml`, and `wiki/index.md`.
2. Search the wiki for the user's topic.
3. Read relevant pages, ADRs, and `repos.yaml` source entries.
4. If the question is operational or engineering-related, include `wiki/engineering/`.
5. If the question is unresolved or speculative, check `inbox/open-questions.md` and `inbox/claims.md`.
6. Answer with links to relevant files.

## Answer Rules

- Distinguish:
  - stable source-backed knowledge
  - wiki synthesis
  - source-needed claims
  - open questions
- Do not invent missing details.
- Mention when the wiki does not contain enough evidence.
- Prefer concise answers with file references.

## Useful Searches

- `rg -n "topic" wiki/ inbox/ CONCEPT.md repos.yaml`
- `rg -n "source-needed|Status: open|Open" inbox/claims.md inbox/open-questions.md`
