# Agent Skills

This package includes reusable skills for maintaining an LLM-friendly cross-repo wiki and running an agent-driven development workflow on top of it. Each skill lives in `.agents/skills/<skill-name>/SKILL.md`.

Agents should use these skills when the user's request matches the skill description, or when the user names the skill directly.

## Included Skills

### Idea → Planning

| Skill | Use When |
|---|---|
| `brainstorm` | Exploring a problem or idea before committing to a plan. Surfaces goals, constraints, options, and open questions. Saves to `inbox/dump/` on request. |
| `planning` | Turning an implementation intent (or a `brainstorm` output) into one or more detailed phase specs in `plans/<feature-slug>/`. Three-phase gate: (1) propose breakdown and pause, (2) write specs at `status: wip`, (3) flip a phase to `status: ready to ship` after every open question is resolved or routed. |

### PR Workflow

| Skill | Use When |
|---|---|
| `evaluate` | Pre-PR gate. Reads `plans/<feature-slug>/`, maps each acceptance criterion to code, reports `complete` / `partial` / `missing` / `unclear`. Skips phases still at `status: wip`. Does **not** run lint/typecheck/UI; does **not** touch the wiki. |
| `review-pr` | Pre-merge. Runs the repo's validation commands (lint, format, typecheck, UI as applicable). Auto-detects a linked GitHub issue. Writes the PR title, body, change breakdown, and total score, then applies via `gh`. No ADR drift detection — that work moved to `/wiki-sync`. Invokable from any repo. |
| `wiki-sync` | Post-merge sync. **PR mode**: creates the ADR (or flips an existing one to `accepted`), updates affected wiki pages, appends a log entry, records in `synced-prs.md` (idempotent), and deletes the matching plan phase file. **Doc mode**: ingests existing implemented knowledge from a file path. Also covers inbox curation (replaces the older `wiki-digest-fragments`). |

### Wiki Retrieval & Maintenance

| Skill | Use When |
|---|---|
| `wiki-query` | Read-only. Searches wiki pages, ADRs, and `repos.yaml`. Checks `inbox/open-questions.md` and `inbox/claims.md` for unresolved items. Cites sources and distinguishes stable knowledge from uncertainty. |
| `wiki-lint` | Audits wiki health. Flags inbox-shaped pages in `wiki/`, dangling links, stale `Last reviewed` dates, missing backlinks, `repos.yaml` feeds that don't resolve, and engineering pages missing runbook links. |
| `wiki-adr` | Creates or amends an ADR for an ad-hoc decision that didn't go through the normal `/planning → /wiki-sync` flow. In the standard flow, `/wiki-sync` creates the ADR post-merge. |
| `wiki-runbook` | Documents cross-repo operational procedures, debugging playbooks, recovery steps, or incidents into `wiki/engineering/`. |

### Typical Workflow Sequence

```
/brainstorm → /planning → implement → /evaluate → /review-pr → merge → /wiki-sync
```

Each skill owns one verb: **/evaluate** verifies (code ↔ plan), **/review-pr** validates and packages (lint/typecheck/UI + PR title/body/score), **/wiki-sync** ingests (ADR + wiki pages + plan cleanup).

For ongoing inbox hygiene, skim `inbox/fragments.md` periodically and `/brainstorm` promising ideas (or delete dead ones). For periodic health, run `/wiki-lint`.

## General Guidance

- Read only the skill needed for the task.
- Start with `CONCEPT.md`, `repos.yaml`, and `wiki/index.md` when the skill asks for them.
- Preserve source authority; wiki pages synthesize, source docs remain authoritative.
- Prefer links and concise summaries over copying source docs wholesale.
- Track uncertainty in `inbox/open-questions.md`.
- Track unsupported but important assertions in `inbox/claims.md`.
- Append meaningful maintenance entries to `wiki/logs/index.md`.
- Update `wiki/index.md` when adding a new page.
- Update `repos.yaml` when introducing a new authoritative source.

## Installing In Another Agent Environment

Copy `.agents/skills/` into the agent-specific skills directory (e.g. `.cursor/skills/`, `.claude/skills/`, `.codex/skills/`), or configure the agent to discover skills from this repo.

If your tool uses a different directory convention, keep each skill folder intact:

```txt
wiki-query/
└── SKILL.md
```

The `name` and `description` frontmatter fields are the discovery surface. Keep them clear and generic when adapting this package for a team.
