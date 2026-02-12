# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview
A modern Astro.js blog focused on AI and software development, built with Tailwind CSS v4, TypeScript, and configured for GitHub Pages deployment. Features RSS feeds, SEO optimization, dark/light theme switching, and responsive design optimized for 2K+ displays.

## Architecture & Structure

### Core Stack
- **Framework**: Astro.js 5.9.3 with TypeScript
- **Runtime**: Bun 1.x
- **Styling**: Tailwind CSS v4 with CSS-first configuration
- **Content**: Astro Content Collections with MDX
- **Deployment**: GitHub Pages with custom base path
- **Fonts**: Inter via rsms.me, JetBrains Mono via fontshare

### Key Directories
- `src/content/posts/` - Blog posts in MDX format
- `src/images/blog/` - Blog post images referenced in frontmatter
- `src/components/` - Reusable Astro components
  - `global/` - Site-wide components (Navigation, Footer)
  - `entries/` - Content display components (BlogEntry)
- `src/layouts/` - Page layouts (BaseLayout, BlogLayout)
- `src/pages/` - File-based routing
- `src/styles/global.css` - Tailwind v4 configuration and theme

### Content Schema
Defined in `src/content/config.ts` with required frontmatter:
```typescript
{
  title: string
  pubDate: Date
  description: string
  author: string
  image: { url: ImageAsset, alt: string }
  tags: string[]
}
```

### Design System
- **Responsive scaling**: Font sizes scale at 2000px+ and 2560px+ breakpoints
- **Color system**: OKLCH-based custom colors with CSS variables
  - Light/dark themes with accent color switching
  - Semantic colors: `--color-bg`, `--color-text`, `--color-accent`
- **Typography**: Inter font family with JetBrains Mono for code

## Development Commands

### Package Management
```bash
bun install              # Install dependencies
bun update              # Update all dependencies
```

### Development
```bash
bun run dev             # Start dev server at localhost:4321
bun run build           # Build for production to ./dist/
bun run preview         # Preview production build locally
bun run astro --help    # Astro CLI help
```

### Deployment
```bash
bun run predeploy       # Build before deployment
bun run deploy          # Deploy to GitHub Pages
```

## Configuration Files

### Critical Settings
- `astro.config.mjs` - Site URL, base path, integrations
- `src/styles/global.css` - Tailwind configuration, themes, responsive scaling
- `src/content/config.ts` - Content collection schema

### Base Path Configuration
The site uses GitHub Pages with base path `/Syntax-Synapse/` - all internal links must use this prefix.

## Content Workflow

### Creating New Posts
1. Add MDX file to `src/content/posts/`
2. Include all required frontmatter fields
3. Add hero image to `src/images/blog/`
4. Reference image in frontmatter: `image: { url: "../images/blog/filename.jpg", alt: "description" }`

### Image Handling
- Blog images: `src/images/blog/`
- Optimized via Astro's Image component
- Responsive sizing with aspect-12/7 ratio

### Tag System
- Tags are automatically collected from all posts
- Dynamic tag pages at `/tags/[tag]`
- Tag cloud displayed on homepage

## SEO & Meta
- **RSS**: Auto-generated at `/rss.xml`
- **Sitemap**: Auto-generated via @astrojs/sitemap
- **Meta tags**: Comprehensive SEO via @astrolib/seo
- **Open Graph**: Configured for social sharing
- **Twitter Cards**: Large image format

## Theme System
- **Dark/Light toggle**: Persistent via localStorage
- **System preference detection**: Respects OS theme
- **CSS variables**: Theme colors defined in global.css
- **Font scaling**: Responsive typography for high-resolution displays