# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview
Astro.js blog starter using Tailwind CSS v4, configured for a microblog with RSS feeds and SEO optimization. Uses Bun as package manager.

## Architecture
- **Framework**: Astro.js (v5.9.3) with TypeScript
- **Runtime**: Bun
- **Styling**: Tailwind CSS v4 with CSS-first approach using `@import "tailwindcss"`
- **Content**: Astro Content Collections with MDX support
- **SEO**: @astrolib/seo for meta tags and structured data
- **RSS**: @astrojs/rss for feed generation
- **Sitemap**: @astrojs/sitemap for search engine discovery

## Key Patterns
- **Content**: Schema defined in `src/content/config.ts` with posts in `src/content/posts/`
- **Images**: Blog images go in `src/images/blog/`, referenced in frontmatter
- **Layouts**: BaseLayout for general pages, BlogLayout for posts
- **Routing**: File-based with dynamic tag pages at `/tags/[tag]`
- **Feeds**: RSS at `/rss.xml`, sitemap auto-generated

## Development Commands
```bash
bun install              # Install dependencies
bun run dev              # Start dev server at localhost:4321
bun run build            # Build for production
bun run preview          # Preview production build locally
bun run astro --help     # Astro CLI help
```

## Configuration
- Update site URL in `astro.config.mjs` before deployment
- Tailwind v4 uses `src/styles/global.css` for configuration
- Posts require: title, pubDate, description, author, image (url+alt), tags[]