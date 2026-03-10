---
name: blog-writer
description: Write a complete, production-quality blog post. Use this skill when asked to write a blog post, article, guide, or tutorial. Mandatory workflow: research all relevant sources first (web, official docs, real examples), create an SVG cover image, write the post with proper frontmatter. Triggers on "write a blog post", "create an article", "write a guide about", "blog about", "new blog post".
---

# Blog Writer

Write production-quality blog posts. Every post requires: mandatory multi-source research, a generated cover image, and correct frontmatter.

## Project Structure

Adapt paths to your project's conventions. A typical structure:

```
blog-posts/
└── <slug>.md                          # Blog post markdown

public/images/blog/
└── <slug>/
    ├── cover.svg (or cover.png)       # Cover image — REQUIRED
    └── <other-images>                 # Inline images referenced in post
```

See [references/frontmatter.md](references/frontmatter.md) for the full frontmatter spec.

---

## Step 1 — Research (Mandatory)

Before writing a single word, research the topic thoroughly. Use ALL applicable sources:

### Source types to check
- **Official documentation** — primary source for any technology topic
- **Official blogs / changelogs** — announcements, guides from the project itself
- **skills.sh / GitHub** — for AI skills and tooling topics
- **Real working examples** — GitHub repos, live demos, open-source code
- **Community discussion** — developer forums, Hacker News, Reddit threads
- **Existing related posts** — check your `blog-posts/` directory to avoid overlap and cross-reference

### How to research
Use `fetch_webpage` for each source. Fetch the actual content — not summaries. Collect:
1. Concrete facts, numbers, commands, API signatures
2. Real code examples that work
3. Common mistakes / gotchas developers hit
4. Links to use in the Resources section

### Research quality check
Ask yourself:
- Can I cite a specific version, date, or number for each claim?
- Do I have at least one real code example per major concept?
- Have I read at least 3 distinct sources?
- Do I know what the reader will get wrong without this post?

---

## Step 2 — Plan the Post

Before writing, decide:

| Decision | Options |
|----------|---------|
| **Category** | `project` · `findings` · `tutorial` · `opinion` |
| **Slug** | `kebab-case-title` — lowercase, hyphens, no special chars |
| **Featured** | `true` if it's a flagship post, `false` otherwise |
| **Tags** | 3–6 tags, lowercase, specific (e.g. `nextjs` not `web`) |
| **Cover concept** | What visual metaphor captures the topic? |

---

## Step 3 — Create the Cover Image

Every post **must** have a cover image at `public/images/blog/<slug>/cover.svg`.

Create a professional SVG cover image. Design principles:
- **Dark background** — `#0f172a` (slate-900) or `#0a0a0a` works well
- **Accent color** — pick one that matches the topic (blue for AI, green for Node, orange for performance, etc.)
- **Hero element** — one dominant visual (icon, diagram, terminal window, abstract shape)
- **Title text** — include the post title or a short key phrase in the SVG
- **Dimensions** — `1200×630` (OG image ratio)
- **No fonts outside system fonts** — use `monospace`, `sans-serif`, or `system-ui`

### Cover SVG template

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1200 630" width="1200" height="630">
  <defs>
    <linearGradient id="bg" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#0f172a"/>
      <stop offset="100%" style="stop-color:#1e293b"/>
    </linearGradient>
  </defs>
  <!-- Background -->
  <rect width="1200" height="630" fill="url(#bg)"/>
  <!-- Accent line -->
  <rect x="0" y="0" width="6" height="630" fill="#<ACCENT>"/>
  <!-- Hero visual — adapt per topic -->
  <!-- ... -->
  <!-- Title -->
  <text x="80" y="520" font-family="system-ui, sans-serif" font-size="52"
        font-weight="700" fill="white">Your Title Here</text>
  <!-- Subtitle / tag -->
  <text x="80" y="575" font-family="monospace" font-size="22"
        fill="#<ACCENT>" opacity="0.9">subtopic · yourblog.com</text>
</svg>
```

Customise the hero visual for the topic. For coding/tech posts, use:
- Terminal window mockup with relevant code
- Abstract geometric patterns (circuit board lines, node graphs, grids)
- Icon/logo representation in geometric style
- Data flow diagrams

---

## Step 4 — Write the Post

### Frontmatter (required)
See [references/frontmatter.md](references/frontmatter.md) for the full schema. Minimum required:

```yaml
---
title: Your Post Title
author: Author Name
pubDatetime: YYYY-MM-DDTHH:MM:SS.000Z
slug: your-post-slug
featured: false
draft: false
tags:
  - tag1
  - tag2
ogImage: ./cover.svg
description: 150-160 char meta description with primary keyword
category: findings
---
```

> Adjust the `ogImage` path to match your project's image URL convention. See [references/frontmatter.md](references/frontmatter.md).

### Post structure

**1. Hook (2–3 sentences)**
Open with the problem, a surprising stat, or a question. No "In this post, I will..." intros.

**2. Context — Why This Matters**
One short section (no H2 header needed). Sets the stage, names the pain.

**3. Main Content (3–5 H2 sections)**
Each H2 covers one concept. Rule: one idea per section, code example where relevant.
- Use fenced code blocks with language tags
- Use `**bold**` for key terms on first use
- Use tables for comparisons, reference data, option lists
- Use inline images for diagrams: `![alt](../images/blog/<slug>/diagram.svg)`

**4. Practical Application**
Step-by-step instructions, commands, real workflow. This is what makes the post bookmarkable.

**5. Conclusion**
3-bullet summary max. End with a link, a command to run, or a next step — not a generic "thanks for reading".

**6. Resources section**
List every source used in research. Format:
```markdown
## Resources

- [Title](URL) — one-line description
```

### Writing rules
- No `I will show you`, `In this article`, `Let's dive in` openers
- No passive voice on technical steps — use imperative: `Run this`, `Set the flag`, `Add the field`
- Contractions are fine (`don't`, `it's`) — sounds human
- Code blocks for all commands, configs, and code snippets
- Spell out acronyms on first use: `Model Context Protocol (MCP)`
- Use real version numbers, real URLs, real output — never placeholder data

---

## Step 5 — Inline Images

For any diagram, architecture illustration, or annotated screenshot, create a matching SVG in `public/images/blog/<slug>/`.

Reference it in markdown:
```markdown
![Description of diagram](../images/blog/<slug>/diagram-name.svg)
```

Good candidates for inline images:
- Architecture/flow diagrams
- Concept visualisations (like the 3-level context loading diagram)
- Annotated terminal output
- Comparison tables that benefit from visual layout
- Step-by-step numbered diagrams

---

## Quality Checklist

Before finishing, confirm:

- [ ] 3+ sources researched and cited in Resources section
- [ ] Cover image created at `public/images/blog/<slug>/cover.svg`
- [ ] `ogImage` field in frontmatter points to the cover
- [ ] All code blocks have language tags (` ```bash `, ` ```ts `, etc.)
- [ ] No placeholder versions, fake URLs, or made-up output
- [ ] Slug is URL-safe (lowercase, hyphens only)
- [ ] Description is 150–160 characters

---

## Common Mistakes to Avoid

| Mistake | Fix |
|---------|-----|
| `ogImage` path doesn't match project convention | Check how your project resolves image URLs |
| Hardcoded version numbers without checking docs | Look them up during research |
| Generic cover (gradient + text only) | Add a hero visual element |
| Missing Resources section | Always credit sources |
| `draft: true` after finishing | Set to `false` when ready to publish |
| Slug with spaces or caps | `my-post-title` not `My Post Title` |
