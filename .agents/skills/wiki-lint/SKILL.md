---
name: wiki-lint
description: Use for periodic maintenance audits of an LLM wiki. Finds stale pages, orphan pages, missing backlinks, broken links, source-needed claims, contradictions, old open questions, and engineering pages that need review after infrastructure, dependency, or repo-structure changes.
---

# Wiki Lint

Use this skill to audit wiki health.

## Workflow

1. Read `CONCEPT.md`, `sources/index.md`, and `wiki/index.md`.
2. Inspect the wiki structure.
3. Check for:
   - pages missing from `wiki/index.md`
   - links to missing files
   - source-needed claims
   - stale `Last reviewed` dates
   - open questions older than the current maintenance cycle
   - engineering pages missing runbook or incident links
   - decisions that should become ADRs
   - contradictions across product/system/engineering pages
4. Report findings first, ordered by severity.
5. Make small obvious fixes when requested or clearly implied.

## Useful Commands

- `find . -name "*.md" | sort`
- `rg -n "source-needed|Status: open|TODO|TBD|Last reviewed" .`
- `rg -n "\\]\\([^)]*\\)" .`

## Output

Use code-review style: findings with file references, open questions, suggested fixes, and optional summary.
