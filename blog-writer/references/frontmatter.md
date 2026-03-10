# Blog Post Frontmatter Reference

Common frontmatter schema for markdown-based blog posts. All fields below.

## Full Schema

```yaml
---
title: "Post Title — Subtitle if needed"
author: Your Name
pubDatetime: YYYY-MM-DDTHH:MM:SS.000Z # ISO 8601, creation date
modDatetime: YYYY-MM-DDTHH:MM:SS.000Z # ISO 8601, last modified (optional)
slug: post-slug-here # URL-safe, kebab-case, unique
featured: false # true = shown in featured section
draft: false # true = not published
tags:
  - tag1
  - tag2
  - tag3
ogImage: ./cover.svg # OG + cover image path (adjust to your project)
description: "150-160 character meta description with primary keyword. Describes what the reader will learn."
category: findings # see categories below
---
```

## Category Values

| Category   | When to use                                                |
| ---------- | ---------------------------------------------------------- |
| `project`  | A project you built — walkthrough, architecture, decisions |
| `findings` | Something discovered, learned, or explored                 |
| `tutorial` | Step-by-step how-to guide for other developers             |
| `opinion`  | Thought leadership, takes, analysis                        |

Default to `findings` when unsure.

## ogImage Path

Set `ogImage` to the path your project uses to serve the cover image. Common patterns:

| Project convention             | Example value                                    |
| ------------------------------ | ------------------------------------------------ |
| Files in `public/images/blog/` | `./cover.svg` or `/images/blog/<slug>/cover.svg` |
| Astro assets                   | `../../assets/images/<slug>/cover.svg`           |
| Next.js public folder          | `/images/blog/<slug>/cover.svg`                  |

The physical file should live wherever your static assets are served from (e.g. `public/images/blog/<slug>/cover.svg`).

## Tag Conventions

Use lowercase, no spaces, specific:

```yaml
# Good
tags:
  - nextjs
  - typescript
  - supabase
  - auth

# Bad
tags:
  - JavaScript   # should be lowercase
  - web dev      # no spaces
  - coding       # too generic
```

## Slug Rules

- Lowercase only
- Hyphens between words (no underscores, no spaces)
- No special characters (`&`, `+`, `/`, etc.)
- 3–6 words is ideal
- Must be unique across all posts

```
# Good slugs
claude-skills-guide
nextjs-supabase-auth
building-mcp-servers

# Bad slugs
My Post Title
nextjs_supabase_auth
post1
```

## Description Rules

- 150–160 characters
- Include the primary keyword naturally
- Describe outcome/benefit, not just topic
- No "In this post"

```
# Good (158 chars)
"Master Claude Skills — build custom skills like daily-summary, install the best ones, and supercharge GitHub Copilot in VS Code with reusable AI workflows."

# Bad
"A post about Claude Skills and how to use them."
```

## pubDatetime Format

Always use ISO 8601 with milliseconds and Z suffix:

```
YYYY-MM-DDTHH:MM:SS.000Z
```

Use today's date for `pubDatetime`. Add `modDatetime` only when updating a published post — otherwise omit it.

Use `modDatetime` if the post was updated after first publish. Otherwise omit it.
