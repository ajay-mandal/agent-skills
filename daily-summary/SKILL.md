---
name: daily-summary
description: Generates a concise engineering notes-style summary of git commits for a given day, filtered to a specific author. Use this skill whenever the user says "daily summary", "what did I do today/yesterday", "give me today's notes", "recap my commits", "summarize my work today", or invokes the skill by name. Trigger proactively anytime the user asks about what they worked on or accomplished in git on a given day.
---

# Daily Summary

Generate a concise, engineering-notes-style summary of git commits for a given day.

## Step 1: Resolve the target date

- Default to **today** if no date is specified.
- Recognise "yesterday", explicit dates like "March 3", "2026-03-03", or "last Monday".
- Convert to the format `YYYY-MM-DD` for use in git commands.

## Step 2: Collect commits

Run this command from the repo root (or each repo root if the workspace is a monorepo):

```bash
git log \
  --after="<DATE> 00:00:00" \
  --before="<DATE> 23:59:59" \
  --author="$(git config user.name)" \
  --format="%H %s" \
  --no-merges
```

Author filter defaults to `$(git config user.name)` — the current repo's configured author. If the user specifies a different author or email, use that instead. Never include other contributors.

If there are no commits, output: `No commits found for <date>.` and stop.

## Step 3: Fetch commit details

For every commit hash returned above, run:

```bash
git show <hash> --stat --format="%B"
```

This gives the full commit message body **and** the list of changed files. Derive all summary content from this output — never guess or infer intent beyond what the diff and message state.

## Step 4: Classify changes into thematic buckets

Read the changed file paths and commit message text, then assign each commit to the most fitting bucket:

| Bucket | Signals |
|---|---|
| **Embeddings / Vector** | embedding services, vector documents, `MongoDBVectorService`, `*embedding*`, `*vector*` |
| **MCP fixes** | `src/mcp/` guards, tools, schemas, constants, `*mcp*` |
| **Intake / Pipeline** | intake controller/service, FTP services, queue modules, BullMQ, `*intake*`, `*queue*` |
| **Prisma / Migrations** | `.sql` migration files, `*.prisma`, model changes, `prisma/` |
| **Auth / Guards** | Firebase guard, RBAC guards, `*auth*`, `*guard*` |
| **Testing / Docs** | `*.spec.ts`, `TESTING.md`, Postman collections, `*.md` docs |
| **Infra / Build** | `tsconfig`, `.gitignore`, `turbo.json`, `pnpm-lock.yaml`, cache or config files |

**Merge minor chore/build items** (single-line bumps, lockfile updates) into the nearest related bucket rather than giving them their own line.

A commit can contribute to multiple buckets if it touches genuinely distinct concerns — split it accordingly.

## Step 5: Write the summary

Output **exactly 3 bold-headed bullet lines** (max 4 if the day was clearly dense — 10+ commits or 4+ distinct meaningful themes).

Format each line:

> **\<Theme\>** — \<what was built/fixed/changed and why it matters (1–2 sentences)\>.

Rules:
- **Factual and terse** — no filler phrases, no "we", no "I", no "the team"
- Write as **engineering notes**, not prose or a changelog
- File and service names in **backticks** (e.g., `McpRbacGuard`, `intake.service.ts`)
- Stick to what the diff and commit messages actually say
- If a bucket has only trivial chore commits, absorb it rather than force a line

## Example output

```
**MCP fixes** — Corrected tenant scoping in `McpRbacGuard` and added missing `@ToolRoles` decorators to three inventory tools; guards now enforce role checks on every tool call.

**Intake / Pipeline** — Wired `FtpPollerService` into BullMQ queue; FTP files now enqueue automatically on poll interval instead of requiring a manual trigger.

**Prisma / Migrations** — Added `ingestion_job` model and migration `20260303_add_ingestion_job`; supports per-tenant job tracking with status and error fields.
```
