# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview
This is an Astro.js blog starter template using Tailwind CSS v4, configured for a microblog with RSS feeds and SEO optimization.

## Architecture
- **Framework**: Astro.js (v5.9.3) with TypeScript
- **Styling**: Tailwind CSS v4 using new CSS-first approach with `@import "tailwindcss"`
- **Content**: Astro Content Collections with MDX support
- **SEO**: @astrolib/seo for meta tags and structured data
- **RSS**: @astrojs/rss for feed generation
- **Sitemap**: @astrojs/sitemap for search engine discovery

## Key Directories
- `src/content/posts/` - Blog posts in markdown format
- `src/components/` - Reusable Astro components
- `src/layouts/` - Page layouts (BaseLayout, BlogLayout)
- `src/pages/` - File-based routing
- `src/styles/global.css` - Global styles with Tailwind imports

## Development Commands
```bash
npm install              # Install dependencies
npm run dev              # Start dev server at localhost:4321
npm run build            # Build for production
npm run preview          # Preview production build locally
npm run astro --help     # Astro CLI help
```

## Content Structure
Posts use schema defined in `src/content/config.ts` with required fields:
- title, pubDate, description, author
- image (url + alt text)
- tags (array of strings)

## Configuration Notes
- Tailwind CSS v4 uses CSS imports instead of config files
- Site URL needs updating in `astro.config.mjs` before deployment
- Images for blog posts go in `src/images/blog/`
- RSS feed available at `/rss.xml`
- Tags system with individual tag pages at `/tags/[tag]`