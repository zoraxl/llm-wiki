# LLM Wiki Concept

Status: template
Last reviewed: YYYY-MM-DD

## Purpose

This wiki is a reusable knowledge system for teams working across multiple repositories, services, products, or workstreams.

It preserves:

- stable implementation knowledge
- evolving product and domain intuition
- cross-repo architecture
- operational procedures
- decisions and their consequences
- open questions and uncertainty
- source-backed claims

The wiki is not the source of truth for every implementation detail. It is the synthesis layer over authoritative source material.

## Core Ideas

- Source - an authoritative repo doc, PR, design note, incident, meeting note, or external reference.
- Inbox - raw material that has not been organized yet.
- Fragment - a rough idea, observation, or partial synthesis.
- Claim - a statement that may be true but needs confirmation or a stronger source.
- Open question - a useful uncertainty the team should track.
- Thread - a continuing line of investigation or product/system thinking.
- Wiki page - synthesized knowledge organized for retrieval.
- ADR - an architecture decision record for durable choices.
- Runbook - operational steps for recurring procedures or incidents.

## Knowledge Layers

1. Raw sources - repo docs, PRs, tickets, meeting notes, and stable external references.
2. Inbox - rough notes, screenshots, chats, and speculative ideas.
3. Wiki synthesis - organized pages that explain the system across boundaries.
4. Decisions - ADRs and durable commitments that future work should respect.

## Maintenance Principles

- Preserve raw source authority; synthesize without pretending synthesis is the source.
- Keep product and domain intuition visible, especially when it is not yet formalized.
- Treat uncertainty as useful knowledge.
- Prefer explicit open questions over confident rewrites.
- Promote decisions into ADRs when they begin constraining implementation.
- Keep pages cross-linked and source-backed where possible.
- Record meaningful maintenance in [wiki/log.md](wiki/log.md).
- Use the reusable skills in [.agents/skills](.agents/skills) for consistent query, ingest, lint, ADR, runbook, and fragment workflows.

## What Belongs Here

Good fits:

- cross-repo architecture
- product concept and vocabulary
- dependency and integration decisions
- infrastructure overview
- deploy and environment knowledge that spans repos
- runbooks for cross-repo operations
- incidents with lasting lessons
- ADRs
- open questions
- claims needing source review
- rough fragments that are not ready for final docs

Poor fits:

- every small bug fix
- every component refactor
- routine dependency patch bumps
- docs that only explain one repo's local code
- copied source docs that already live in an owning repo

## Recommended Page Lifecycle

1. Capture raw notes in `inbox/`.
2. Link source material in [sources/index.md](sources/index.md).
3. Promote durable knowledge into `wiki/`.
4. Track uncertainty in [wiki/open-questions.md](wiki/open-questions.md).
5. Track source-needed statements in [wiki/claims.md](wiki/claims.md).
6. Promote durable choices into [wiki/decisions/](wiki/decisions/).
7. Append meaningful updates to [wiki/log.md](wiki/log.md).

## Reusable Skills

The package includes agent-readable skills in [.agents/skills](.agents/skills):

- `wiki-query` for retrieval.
- `wiki-ingest` for adding durable source material.
- `wiki-digest-fragments` for inbox maintenance.
- `wiki-lint` for health checks.
- `wiki-adr` for decisions.
- `wiki-runbook` for operations.

When adapting this template, keep skill descriptions generic enough to work across teams, but update examples or section names if your wiki structure changes.

## Customization Checklist

- Replace placeholder repo names in [sources/index.md](sources/index.md).
- Decide which wiki sections your team needs.
- Rename or delete unused example pages.
- Add links to your repo-local docs.
- Add your first maintenance log entry.
- Add project-specific vocabulary to [wiki/product/glossary.md](wiki/product/glossary.md).
