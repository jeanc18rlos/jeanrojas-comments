# jeanrojas-comments

Canonical home for **content + comments** powering [jeanrojas.com](https://jeanrojas.com).

## Layout

```
posts/                # MDX articles. Synced into the Next.js app at build time.
  *.mdx               # one file per blog post (frontmatter + body)
```

## How it works

- **Content** — MDX files in `posts/` are the source of truth. The
  [jeanrojas.com](https://github.com/jeanc18rlos/jeanrojas.com) Next.js app
  pulls them in at build time via `scripts/sync-content.mjs` (configured
  through the `CONTENT_REPO` env var, which defaults to this repo).
- **Comments** — Every post slug maps to a GitHub Discussion in the `Comments`
  category here. The site mounts [Giscus](https://giscus.app) on each blog
  page, so reader comments end up as Discussion replies — visible on the post
  page itself and on github.com side by side with the source MDX.

## Workflow

1. Write a new MDX file under `posts/` with the standard frontmatter:
   ```mdx
   ---
   title: "..."
   description: "..."
   date: "YYYY-MM-DD"
   tags: ["..."]
   author: "Jean Rojas"
   ---
   ```
2. Push to `main`. A Vercel deploy hook on the comments repo triggers a fresh
   site build, which re-syncs `posts/*.mdx` into `content/blog/`.
3. The first time someone comments, Giscus auto-creates the matching
   Discussion in the `Comments` category.

## Why a separate repo

- Keeps the portfolio repo's Discussions tab clean — discussions here are
  reader-facing, not contributor chatter.
- Content portability — if the engine ever changes, the corpus stays.
- One push = one deploy: writing a new article doesn't touch source code.

## Frontmatter reference

| Field | Required | Notes |
|---|---|---|
| `title` | yes | Used for `<title>`, OG, the post page H1 |
| `description` | yes | Excerpt + meta description + OG |
| `date` | yes | ISO `YYYY-MM-DD` — sorts the index, drives RSS pubDate |
| `tags` | no | array of strings — surfaces in the `/blog` filter |
| `author` | no | defaults to `Jean Rojas` |
| `cover` | no | path to a hero image |
| `coverAlt` | no | alt text for the cover |
| `coverCredit` | no | shown under the hero |
| `updated` | no | ISO date — adds an "updated" timestamp |
| `draft` | no | `true` hides the post in production |
