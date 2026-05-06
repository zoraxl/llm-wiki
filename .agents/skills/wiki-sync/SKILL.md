---
name: wiki-sync
description: Use to sync the wiki from a merged PR or from a source doc/file path. Two modes — PR mode (post-merge: creates ADR if needed, flips ADR to accepted, updates wiki pages, appends log, cleans up related plan files) and doc mode (ingests existing implemented knowledge from a file or doc path directly). Use when the user says "wiki-sync", "/wiki-sync", "sync the wiki", "update the wiki from this PR", "ingest this PR", "ingest this doc", or "add this to the wiki".
---

# Wiki Sync

Two input modes — **PR mode** for post-merge sync, **doc mode** for ingesting existing implemented knowledge that has no PR or predates the workflow.

---

## Mode detection

| Input | Mode |
|-------|------|
| A number, `owner/repo#NNN`, PR URL, or `sha:abc` | **PR mode** |
| A file path, glob, or prose description of a source doc | **Doc mode** |
| No argument | List recent unsynced PRs and prompt — runs PR mode on selection |

---

## PR Mode

Use after a PR merges to the default branch in an implementation repo (any repo registered under `repos[]` in `repos.yaml`).

### Inputs

1. PR number on the default repo: `/wiki-sync 1234` (defaults to the first `role: implementation` repo in `repos.yaml`)
2. Cross-repo PR: `/wiki-sync owner/repo#1234`
3. PR URL: `/wiki-sync https://github.com/owner/repo/pull/1234`
4. Merged commit SHA: `/wiki-sync sha:abc123`
5. No argument: list the most recently merged PRs not yet in `wiki/logs/synced-prs.md`, prompt the user to pick one

### Step 1 — Read context

Read `repos.yaml`, `CONCEPT.md`, `wiki/index.md`, and `wiki/decisions/index.md`.

### Step 2 — Resolve the PR

```bash
gh pr view <pr> --repo <owner/repo> --json title,body,files,mergedAt,labels,state,headRefName,closingIssuesReferences
```

Require `state=MERGED`. If the PR is not yet merged, stop and tell the user. Check `wiki/logs/synced-prs.md` — if the PR URL already appears, report "already synced" and stop (idempotent).

### Step 3 — Identify the bound plan file

Look for a reference to a plan file in this order:

1. A line matching `plans/<feature-slug>/<phase-slug>.md` in the PR body
2. Scan `plans/` for a phase slug that matches the PR title or branch name
3. Ask the user: "Which plan file does this PR correspond to? (e.g. `plans/<feature-slug>/<phase-slug>.md`, or 'none')"

If the user says "none" or no plan file is found, skip plan cleanup at the end.

### Step 4 — Resolve or create the ADR

Look for an existing bound ADR in this order:

1. Any closing issue with an `adr:NNN` label → read `wiki/decisions/adr-NNN-*.md`
2. A line matching `wiki/decisions/adr-NNN-` in the PR body
3. The head branch name matching `^adr-NNN-`

**If an existing ADR is found:** proceed to Step 5 to flip its status.

**If no ADR is found:** create one now.

- Find the highest existing ADR number:
  ```bash
  ls wiki/decisions/adr-*.md | sort | tail -1
  ```
  Increment by 1. Use zero-padded 3-digit format: `adr-001-`, `adr-002-`, etc.

- Derive the slug from the PR title or plan file name (kebab-case).

- Write `wiki/decisions/adr-NNN-<slug>.md`:

  ```markdown
  # ADR NNN: <Title>

  Status: accepted
  Date: YYYY-MM-DD
  Implemented-by: <PR URL>
  Implemented-on: YYYY-MM-DD

  ## Context

  <1-2 sentences on why this was built and what problem it solves — derive from PR body or plan file>

  ## Key Design Decisions

  Bullet list of the meaningful choices made in this phase. Derive from the "Key Design Decisions" section of the plan file. Each bullet should state the choice and the reason behind it.

  - **<Decision>** — <why this approach was chosen over alternatives>
  - …

  ## Alternatives Considered

  <For each rejected alternative: what it was and why it was ruled out. Derive from plan file if available, otherwise "Not recorded.">

  ## Consequences

  <Bullet list of outcomes and trade-offs — what this enables, what it costs, what to watch.>

  ## Sources

  - PR: <PR URL>
  - Plan: `<plan file path if available>`

  ## Related

  - [Decision index](index.md)
  ```

- Update `wiki/decisions/index.md` — add a row for the new ADR.

### Step 5 — Update ADR status (if ADR already existed)

In `wiki/decisions/adr-NNN-<slug>.md`:

- Change `Status: proposed` → `Status: accepted`
- Add after the status line:
  - `Implemented-by: <PR URL>`
  - `Implemented-on: YYYY-MM-DD`

### Step 6 — Identify affected wiki pages

```bash
gh pr view <pr> --repo <owner/repo> --json files --jq '.files[].path'
```

For each changed file, look up `repos[name].sources[path].feeds` in `repos.yaml` to find the downstream wiki pages.

### Step 7 — Update affected wiki pages

For each wiki page from Step 6: read the current page, add or update a brief synthesis section. Do not paste the diff wholesale. Preserve source authority with a link or path. Mark uncertainty explicitly.

### Step 7.5 — Write Tunable Knobs to engineering wiki

If the plan file has a "Tunable Knobs and Notes" section with any content, append it to `wiki/engineering/tunable-knobs.md` (create the file if it doesn't exist):

```markdown
## <ADR title> (ADR NNN)

> Source: `<plan file path>` — ingested YYYY-MM-DD

<Paste the Tunable Knobs section verbatim from the plan file>
```

This preserves operational knowledge (thresholds, caps, timeouts, signals to watch) that would otherwise be lost when the plan file is deleted. If there are no tunable knobs, skip this step.

### Step 8 — Append to wiki log

Append to `wiki/logs/index.md`:

```md
### YYYY-MM-DD — <ADR title or PR title>

- PR: <PR URL>
- ADR: <ADR path> (status → accepted)
- Wiki pages updated: <list>
- Sources consulted: <list of changed source files>
```

If `wiki/logs/index.md` exceeds ~20 entries, move older entries to `wiki/logs/YYYY-MM.md` and keep the last 5 in `index.md`.

### Step 9 — Record in synced-prs ledger

Append to `wiki/logs/synced-prs.md` (create if missing):

```
<PR URL> synced <YYYY-MM-DD>
```

### Step 10 — Clean up plan files

If a plan file was identified in Step 3:

1. **Soft warning — sibling phases still wip.** Before deleting, scan all sibling phase files in `plans/<feature-slug>/` for `status: wip` in the frontmatter. If any are found, print a warning (do **not** block):

   > **Heads-up:** the following phases under `plans/<feature-slug>/` are still `status: wip` and were never reviewed: `<list of files>`. They will remain in `plans/` after cleanup. If they were intentionally abandoned (scope changed, superseded), consider removing them or moving outstanding ideas to `inbox/backlog.md`. Proceeding with cleanup of the implemented phase.

2. **Soft warning — unresolved tests.md entries.** If `plans/<feature-slug>/tests.md` exists, scan it for entries with `Status: open`. If any are found, print a warning:

   > **Heads-up:** `plans/<feature-slug>/tests.md` has open entries that were not resolved during implementation: `<list of question titles>`. If the feature folder is about to be deleted (last phase shipped), consider migrating them to `inbox/backlog.md` or to a relevant wiki engineering note before they disappear. Proceeding.

3. Delete the phase file:
   ```bash
   rm plans/<feature-slug>/<phase-slug>.md
   ```

4. Check if the feature folder is now empty (ignoring `tests.md` for the empty check — if only `tests.md` remains, treat as empty and delete the folder along with it; the prior warning gave the user a chance to migrate any open entries):
   ```bash
   ls plans/<feature-slug>/
   ```

5. If only `tests.md` remains (or the folder is empty of `.md` files), delete the folder:
   ```bash
   rm -r plans/<feature-slug>/
   ```

Print what was deleted, including any `tests.md` removed alongside the folder.

### Step 11 — Output

List:
- ADR path and status (created or flipped to accepted)
- Wiki files updated
- Log entry path
- Plan files deleted (if any)

---

## Doc Mode

Use when ingesting **existing implemented knowledge** from a source doc or file — no PR required. Typical cases:
- A raw fragment in `inbox/` that's now ready to promote
- Existing repo docs that predate the `/planning` workflow
- External material (specs, meeting notes) describing something already shipped

### Inputs

- A file path: `/wiki-sync ../repo-a/docs/architecture/overview.md`
- A glob: `/wiki-sync ../repo-a/docs/*.md`
- A prose description with an attached file reference

### Step 1 — Read context

Read `repos.yaml`, `CONCEPT.md`, `wiki/index.md`.

### Step 2 — Read the source material

Read the provided file(s). Extract:
- New product/system concepts
- Architecture or data-flow facts
- Durable decisions or tradeoffs
- Implementation details that cross repo boundaries

### Step 3 — Identify affected wiki pages

Look up the source path in `repos[name].sources[path].feeds` in `repos.yaml`. If the path isn't registered, determine the best-fit wiki page from context and ask the user to confirm before writing.

If no existing wiki page is relevant, ask whether to create one or add to `inbox/fragments.md` for later promotion.

### Step 4 — Update wiki pages

For each target wiki page: synthesize the new knowledge into the appropriate section. Rules:
- Do not copy source docs wholesale.
- Preserve source authority with a link or path.
- Synthesize; do not mirror.
- Mark uncertainty explicitly.
- If the material affects operations, update `wiki/engineering/runbooks.md`.
- If the material changes cross-repo architecture, update `wiki/system/architecture.md` or `wiki/system/data-flow.md`.
- If the source path isn't already in `repos.yaml`, add it to the correct `repos[name].sources[]` entry.

### Step 5 — Append to wiki log

Append to `wiki/logs/index.md`:

```md
### YYYY-MM-DD — Ingested <source path>

- Source: <file path(s)>
- Wiki pages updated: <list>
- ADR: <if a durable decision was identified, link or "none">
```

### Step 6 — Output

List files touched, sources consulted, log entry path. If a durable decision was found with no ADR, note: "Consider running `/wiki-adr` to record this as a formal decision."

---

## Inbox Curation (Replaces wiki-digest-fragments)

When the user asks to digest or curate the inbox (e.g. "digest fragments", "process inbox", "what's in inbox"), use Doc mode against the relevant `inbox/` files. Rules specific to inbox curation:

- Group fragments by theme before deciding promotion.
- For each fragment, choose one action: leave in inbox, convert to `inbox/open-questions.md`, record in `inbox/claims.md`, promote into an existing wiki page, or suggest an ADR.
- Preserve original wording when phrasing carries product or domain intuition.
- Do not over-polish early ideas. Keep contradictions visible.
- Do not delete raw inbox material unless it is clearly superseded; the wiki log entry should record where it went.

---

## Rules (all modes)

- Do not copy source docs wholesale into the wiki.
- Preserve source authority with links or paths.
- Synthesize; do not mirror.
- Mark uncertainty explicitly.
- Do not present speculation as settled knowledge.
- The wiki contains only decided and implemented knowledge — if material is still speculative, direct it to `inbox/fragments.md` or `inbox/open-questions.md` instead.
