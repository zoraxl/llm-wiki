---
name: wiki-lint
description: Use for periodic maintenance audits of the team wiki. Finds stale pages, orphan pages, missing backlinks, unsourced claims, contradictions, old open questions, and engineering pages that need review after infrastructure, dependency, or repo-structure changes.
---

# Wiki Lint

Use this skill to audit wiki health.

## Workflow

1. Read `CONCEPT.md`, `repos.yaml`, and `wiki/index.md`.
2. Inspect the wiki structure.
3. Check for:
   - pages missing from `wiki/index.md`
   - links to missing files
   - any of `fragments.md`, `open-questions.md`, `claims.md`, `threads.md` existing directly under `wiki/` (violation: these belong in `inbox/`)
   - any `wiki/log.md` (singular) reference — should be `wiki/logs/index.md`
   - stale `Last reviewed` dates
   - engineering pages missing runbook or incident links
   - contradictions across product/system/engineering pages
   - `repos[].sources[].feeds` targets in `repos.yaml` that don't exist as wiki pages
   - `repos[].sources[].path` entries that don't exist in the sibling repo on disk (flag, don't fail)
4. Report findings first, ordered by severity.
5. Make small obvious fixes when requested or clearly implied.

## Useful Commands

- `find . -name "*.md" | sort`
- `rg -n "source-needed|Status: open|TODO|TBD|Last reviewed" .`
- `rg -n "\\]\\([^)]*\\)" .`

## Output

Use code-review style:

- findings with file references
- open questions
- suggested fixes
- optional summary
