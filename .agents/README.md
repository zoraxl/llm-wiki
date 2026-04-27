# Agent Skills

This package includes reusable skills for maintaining an LLM-friendly wiki. Each skill lives in `.agents/skills/<skill-name>/SKILL.md`.

Agents should use these skills when the user's request matches the skill description, or when the user names the skill directly.

## Included Skills

| Skill | Use When |
|---|---|
| `wiki-query` | Answering questions from the wiki. |
| `wiki-ingest` | Adding durable source material, PR summaries, implementation discoveries, or meeting notes. |
| `wiki-digest-fragments` | Organizing rough inbox material without over-polishing it. |
| `wiki-lint` | Auditing wiki health, stale pages, missing links, open questions, and source-needed claims. |
| `wiki-adr` | Recording durable architecture, infrastructure, dependency, repo-boundary, or operations decisions. |
| `wiki-runbook` | Documenting operational procedures, debugging playbooks, deployment checks, or incident recovery steps. |

## General Guidance

- Read only the skill needed for the task.
- Start with `CONCEPT.md`, `sources/index.md`, and `wiki/index.md` when the skill asks for them.
- Preserve source authority; wiki pages synthesize, source docs remain authoritative.
- Prefer links and concise summaries over copying source docs wholesale.
- Track uncertainty in `wiki/open-questions.md`.
- Track unsupported but important assertions in `wiki/claims.md`.
- Append meaningful maintenance to `wiki/log.md`.
- Update `wiki/index.md` when adding a new page.
- Update `sources/index.md` when introducing a new authoritative source.

## Installing In Another Agent Environment

Copy `.agents/skills/` into the agent-specific skills directory, or configure the agent to discover skills from this repo.

If your tool uses a different directory convention, keep each skill folder intact:

```txt
wiki-query/
└── SKILL.md
```

The `name` and `description` frontmatter fields are the discovery surface. Keep them clear and generic when adapting this package for a team.
