---
title: "Adding Light/Dark Theme Switching to Astro with Tailwind CSS v4"
pubDate: 2025-07-28T18:45:00Z
description: "A comprehensive guide to implementing flicker-free dark mode in Astro.js using Tailwind CSS v4, including system preference detection and persistent storage."
author: "Syntax & Synapse"
image:
  url: "../../images/blog/E16AED80-5C46-4770-8927-AA88D7822495_1_105_c.jpeg"
  alt: "Dark mode toggle button with sun and moon icons"
tags: ["astro", "tailwind-css", "dark-mode", "javascript", "web-development"]
---

Dark mode isn't just a trend—it's a user preference that can significantly improve the reading experience, especially for technical content like this blog. In this post, I'll walk you through how we added a seamless light/dark theme switching feature to this Astro blog using Tailwind CSS v4.

## Why We Needed This

After launching this blog, we quickly realized that many developers prefer reading technical content in dark mode. The default light theme was causing eye strain during long reading sessions, and we wanted to provide a better user experience without compromising the clean, minimalist design.

## The Challenge with Tailwind CSS v4

Tailwind CSS v4 introduced some significant changes compared to v3:

- **CSS-first configuration** instead of JavaScript config files
- **New `@custom-variant` syntax** for dark mode variants
- **Improved color system** with semantic variables
- **Better performance** with optimized builds

Let's dive into the implementation.

## Step 1: Configure Tailwind CSS v4 for Dark Mode

First, we need to update our global CSS file to support dark mode. Here's the key configuration:

```css
/* src/styles/global.css */
@import "tailwindcss";
@plugin "@tailwindcss/forms";
@plugin "@tailwindcss/typography";

@custom-variant dark (&:where(.dark, .dark *));

@theme {
  /* Light mode colors */
  --color-base-50: oklch(96.74% 0.001 286.38);
  --color-base-100: oklch(93.73% 0.001 286.37);
  /* ... other base colors ... */
  
  /* Semantic colors for consistent theming */
  --color-bg: var(--color-base-50);
  --color-bg-secondary: var(--color-base-100);
  --color-text: var(--color-base-900);
  --color-text-secondary: var(--color-base-700);
  --color-border: var(--color-base-200);
  --color-accent: var(--color-accent-600);
}

/* Dark mode color overrides */
:root.dark {
  --color-bg: var(--color-base-950);
  --color-bg-secondary: var(--color-base-900);
  --color-text: var(--color-base-100);
  --color-text-secondary: var(--color-base-400);
  --color-border: var(--color-base-700);
  --color-accent: var(--color-accent-400);
}
```

The `@custom-variant dark (&:where(.dark, .dark *))` line tells Tailwind to activate dark mode utilities when the `.dark` class is present on any parent element.

## Step 2: Add the Blocking Theme Script

To prevent the dreaded "flash of unstyled content" (FOUC), we add a blocking script that runs before the page is painted:

```astro
<!-- In src/layouts/BaseLayout.astro -->
<html lang="en">
  <head>
    <BaseHead />
    <!-- This script runs immediately, preventing flicker -->
    <script is:inline>
      (() => {
        const key = 'theme-pref';
        const preference = localStorage.getItem(key) ||
          (window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light');
        document.documentElement.classList.toggle('dark', preference === 'dark');
      })();
    </script>
  </head>
  <!-- ... -->
</html>
```

The `is:inline` directive ensures this script runs synchronously, applying the theme before any content is rendered.

## Step 3: Create the Theme Toggle Component

We built a reusable `ThemeToggle` component with proper accessibility:

```astro
---
// src/components/ThemeToggle.astro
---
<button
  id="theme-toggle"
  class="p-2 rounded-full hover:bg-base-200 dark:hover:bg-base-800 transition-colors duration-200"
  aria-label="Toggle dark mode"
  title="Toggle dark mode"
>
  <!-- Sun icon (visible in light mode) -->
  <svg class="w-5 h-5 text-base-900 dark:hidden" fill="none" stroke="currentColor" viewBox="0 0 24 24">
    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"/>
  </svg>

  <!-- Moon icon (visible in dark mode) -->
  <svg class="w-5 h-5 text-base-100 hidden dark:block" fill="none" stroke="currentColor" viewBox="0 0 24 24">
    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"/>
  </svg>
</button>

<script>
  const themeToggle = document.getElementById('theme-toggle');
  themeToggle?.addEventListener('click', () => {
    window.toggleTheme();
  });
</script>
```

## Step 4: Add the Global Theme Toggle Function

We added a global helper function for theme management:

```html
<!-- In BaseLayout.astro -->
<script>
  window.toggleTheme = () => {
    const root = document.documentElement;
    const isDark = root.classList.toggle('dark');
    localStorage.setItem('theme-pref', isDark ? 'dark' : 'light');
  };
</script>
```

## Step 5: Update All Components for Dark Mode

We systematically updated all components to use semantic color variables:

### Before:
```html
class="text-base-900 bg-white"
```

### After:
```html
class="text-text bg-bg"
```

This approach ensures consistent theming across the entire site and makes future color adjustments much easier.

## Step 6: Add Dark Mode Support to Typography

We enhanced the typography plugin with dark mode support:

```html
<div class="prose prose-sm max-w-md mx-auto dark:prose-invert">
  <!-- Content here automatically adapts to dark mode -->
</div>
```

## The Final Integration

We placed the theme toggle in the navigation bar alongside existing elements:

```astro
<!-- In Navigation.astro -->
<nav>
  <div class="flex gap-3">
    <ThemeToggle />
    <a href="https://github.com/gnokit/Syntax-Synapse">GitHub</a>
  </div>
</nav>
```

## Testing the Implementation

Here's how to test your new dark mode:

1. **Start the development server**: `bun run dev`
2. **Check system preference**: The theme should match your OS setting
3. **Toggle manually**: Click the sun/moon icon in the navigation
4. **Test persistence**: Refresh the page - your choice should be remembered
5. **Check responsiveness**: Verify it works on mobile devices
6. **Test system changes**: Change your OS theme preference and see the site respond

## Advanced Features You Can Add

### 1. Animated Transitions
Add smooth transitions when switching themes:

```css
* {
  transition: background-color 0.2s ease, color 0.2s ease;
}
```

### 2. Keyboard Navigation
Support keyboard theme switching:

```javascript
document.addEventListener('keydown', (e) => {
  if (e.ctrlKey && e.shiftKey && e.key === 'D') {
    e.preventDefault();
    window.toggleTheme();
  }
});
```

### 3. Per-Page Theme Override
Allow individual posts to specify a preferred theme:

```yaml
---
title: "Dark Theme Post"
preferredTheme: "dark"
---
```

## Performance Considerations

- **Zero dependencies**: No external libraries required
- **Minimal JavaScript**: Only ~200 bytes of inline code
- **CSS-first approach**: Leverages Tailwind's optimized CSS generation
- **No hydration issues**: Works seamlessly with Astro's static generation

## Browser Support

This implementation works in all modern browsers that support:
- CSS custom properties
- `localStorage`
- `prefers-color-scheme` media query
- Tailwind CSS v4

## Conclusion

Adding dark mode to this Astro blog was straightforward thanks to Tailwind CSS v4's improved dark mode support. The implementation is lightweight, accessible, and provides a great user experience without any performance overhead.

The theme toggle is now live in the navigation bar - try it out! The choice you make will be remembered across sessions, and the site will respect your system preference if you haven't set a manual preference.

Want to see the code in action? Check out the [GitHub repository](https://github.com/gnokit/Syntax-Synapse) for the full implementation.

---

*Have questions about implementing dark mode in your own Astro project? Drop a comment or reach out on GitHub!*