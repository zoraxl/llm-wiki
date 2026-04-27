# Sources Index

Status: template
Last reviewed: YYYY-MM-DD

Raw source docs stay in their owning repos. This file maps authoritative sources to synthesized wiki pages.

## Repository Ownership

| Repo | Owns |
|---|---|
| `repo-a` | Replace with the code, docs, services, or product areas owned by this repo. |
| `repo-b` | Replace with another owning repo or workstream. |
| `llm-wiki` | Cross-repo synthesis, team memory, ADRs, fragments, and open questions. |

## Source Map

| Source | Feeds |
|---|---|
| `../repo-a/docs/architecture.md` | `wiki/system/architecture.md`, `wiki/system/repo-map.md` |
| `../repo-a/docs/deploys.md` | `wiki/engineering/deploys.md`, `wiki/engineering/runbooks.md` |
| `../repo-b/docs/integration.md` | `wiki/system/data-flow.md`, `wiki/engineering/dependencies.md` |

## Ephemeral Sources

Use `inbox/` for chat notes, screenshots, meeting notes, and rough ideas that do not have a stable repo-local home yet.

## Source Entry Template

```md
### <Source Title>

Local path: ../repo-name/docs/example.md
Link: https://example.com/path
Status: draft | active | deprecated
Feeds:
- wiki/system/architecture.md
- wiki/open-questions.md
Notes:
- Brief note about why this source matters.
```
