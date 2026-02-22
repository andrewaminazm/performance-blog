# performance-blog

A blog focused on **performance testing**: how to start, step-by-step process, **JMeter**, **k6**, **Postman**, and **performance metrics**.

## Quick start

```bash
npm install
npm run dev
```

Open [http://localhost:4321](http://localhost:4321).

## Build

```bash
npm run build
npm run preview
```

## Content plan

See **BLOG-PLAN.md** for:

- Recommended structure and tech (Astro)
- Full topic list and post order
- Definitions of performance metrics
- Suggested categories and navigation

## Adding posts

1. Add a `.md` file in `src/content/blog/`.
2. Use frontmatter: `title`, `pubDate`, `tags`, optional `description`, `draft`.
3. Write in Markdown; use code blocks for JMeter/k6/Postman snippets.

Example:

```yaml
---
title: Your post title
description: Short description for SEO.
pubDate: 2025-02-22
tags: [jmeter, metrics]
draft: false
---
```

## Drafts

Posts with `draft: true` are excluded from the blog list and from production build.
