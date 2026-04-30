# Team-HQ

A reusable, LLM-friendly internal wiki template for teams that need a durable cross-repo knowledge store.

This repository is the synthesis layer. Repo-local docs remain authoritative for implementation details; this wiki explains how the product concept, systems, engineering operations, decisions, and unresolved questions fit together.

Start here:

- [CONCEPT.md](CONCEPT.md) - wiki model, principles, and maintenance rules
- [sources/index.md](sources/index.md) - authority map for raw sources
- [wiki/index.md](wiki/index.md) - table of contents
- [wiki/log.md](wiki/log.md) - append-only maintenance log
- [.agents/README.md](.agents/README.md) - reusable agent skills and usage guidance

## Repository Layout

```txt
llm-wiki/
├── CONCEPT.md
├── sources/
│   └── index.md
├── inbox/
│   ├── chats.md
│   ├── fragments.md
│   └── screenshots.md
├── .agents/
│   ├── README.md
│   └── skills/
└── wiki/
    ├── index.md
    ├── log.md
    ├── claims.md
    ├── fragments.md
    ├── open-questions.md
    ├── threads.md
    ├── product/
    ├── system/
    ├── engineering/
    └── decisions/
```

## How To Use This Wiki

Use this repo as the team's compiled memory for cross-repo product, system, and engineering knowledge.

## Agent Skills

This package includes general-purpose wiki maintenance skills in [.agents/skills](.agents/skills):

- `wiki-query` - retrieve source-backed knowledge from the wiki
- `wiki-ingest` - add durable source material and implementation discoveries
- `wiki-digest-fragments` - organize rough inbox notes
- `wiki-lint` - audit wiki health
- `wiki-adr` - record durable decisions
- `wiki-runbook` - document operational procedures

See [.agents/README.md](.agents/README.md) for discovery and usage guidance.

## Copy-Paste Setup Prompt

Paste this into Claude, Cursor, Codex, or another coding agent after opening this repo:

```txt
Set up this repository as an LLM-maintained cross-repo wiki.

Please read:
- README.md
- CONCEPT.md
- sources/index.md
- wiki/index.md
- .agents/README.md

Then register or follow the reusable skills in .agents/skills:
- wiki-query
- wiki-ingest
- wiki-digest-fragments
- wiki-lint
- wiki-adr
- wiki-runbook

When I ask questions about the wiki, use wiki-query.
When I provide PR summaries, source docs, meeting notes, implementation discoveries, or engineering changes, use wiki-ingest.
When I ask you to process inbox notes, chats, screenshots, or rough ideas, use wiki-digest-fragments.
When I ask for a wiki audit or maintenance pass, use wiki-lint.
When a durable architecture, infrastructure, dependency, repo-boundary, data contract, or operations decision appears, use wiki-adr.
When I ask for operational procedures, debugging steps, deploy checks, or incident recovery docs, use wiki-runbook.

Preserve source authority: repo-local docs and linked sources remain authoritative, and this wiki should synthesize rather than copy them wholesale.
Track uncertainty in wiki/open-questions.md.
Track unsupported but important assertions in wiki/claims.md.
Update sources/index.md when new authoritative sources are introduced.
Update wiki/index.md when new pages are added.
Append meaningful maintenance to wiki/log.md.

First, inspect the current repo and tell me what setup or customization is still needed for this team.
```

For tools that require skills to live in a specific directory, copy each folder from `.agents/skills/` into that tool's skills directory while preserving the `SKILL.md` file inside each folder.

### 1. Before Starting Work

Ask what already exists:

```txt
What do we know about <feature/system>?
Which decisions affect <area>?
What open questions exist around <topic>?
Where are the authoritative source docs for <workflow>?
```

This helps avoid rediscovering context or contradicting an existing decision.

### 2. During Development

Keep implementation-adjacent docs in the repo that owns the code. Use this wiki for:

- cross-repo architecture
- product concept and vocabulary
- durable engineering knowledge
- runbooks and operational procedures
- incidents with lasting lessons
- architecture decision records
- open questions and source-needed claims
- rough ideas that are not ready for final docs

If a rough idea does not belong anywhere yet, add it to [inbox/fragments.md](inbox/fragments.md). If it came from a meeting or chat, add a short summary to [inbox/chats.md](inbox/chats.md).

### 3. Before Opening A PR

Check whether the change affects durable knowledge:

- product concept or terminology
- cross-repo architecture
- data contracts or integration boundaries
- infrastructure, environments, deploys, or secrets
- repo/package ownership
- major dependency choices
- operational procedures
- decisions that future work must respect

Small local implementation changes usually do not need a wiki update.

### 4. During Review

Reviewers can ask:

- Does this PR need a wiki update?
- Does it resolve an open question?
- Does it create or change an architecture decision?
- Does it affect a runbook or incident note?
- Does it contradict a wiki page or ADR?

For larger changes, include a checklist:

```md
- [ ] Checked relevant wiki pages
- [ ] Updated wiki if product/system/engineering knowledge changed
- [ ] Added or updated an ADR if this creates a durable decision
- [ ] Captured unresolved questions in wiki/open-questions.md
- [ ] Updated runbooks if operations changed
```

### 5. After Merge

For meaningful changes, add the PR summary or implementation discovery to the relevant wiki pages and append an entry to [wiki/log.md](wiki/log.md).

For messy accumulated notes, digest the inbox into:

- [wiki/fragments.md](wiki/fragments.md)
- [wiki/open-questions.md](wiki/open-questions.md)
- [wiki/claims.md](wiki/claims.md)
- relevant synthesized pages
- ADRs when a decision becomes durable

## Rule Of Thumb

Repo docs explain how one part works.

This wiki explains how the whole system fits together, why decisions were made, and what the team knows so far.
