# shadcn/ui Theming & Customization Guide

## Overview

This guide covers theme configuration, CSS variables, dark mode implementation, and component customization for shadcn/ui using modern approaches with **Tailwind CSS v4**, **Vue 3**, and **Inertia.js**.

---

## Tailwind CSS v4 Integration

### Theme Configuration with @theme

Tailwind v4 uses a CSS-first approach with the `@theme` directive. Define your shadcn/ui theme in your main CSS file:

```css
/* app.css or global.css */
@import "tailwindcss";

@theme {
  /* Color system using OKLCH for better color perception */
  --color-background: oklch(1 0 0);
  --color-foreground: oklch(0.09 0.005 285.82);
  --color-primary: oklch(0.35 0.1 285.82);
  --color-primary-foreground: oklch(0.98 0.005 285.82);
  --color-secondary: oklch(0.96 0.005 285.82);
  --color-secondary-foreground: oklch(0.35 0.1 285.82);
  --color-muted: oklch(0.96 0.005 285.82);
  --color-muted-foreground: oklch(0.50 0.02 285.82);
  --color-accent: oklch(0.96 0.005 285.82);
  --color-accent-foreground: oklch(0.35 0.1 285.82);
  --color-destructive: oklch(0.58 0.22 29.23);
  --color-destructive-foreground: oklch(0.98 0.005 285.82);
  --color-border: oklch(0.92 0.005 285.82);
  --color-input: oklch(0.92 0.005 285.82);
  --color-ring: oklch(0.09 0.005 285.82);
  
  /* Border radius */
  --radius-lg: 0.5rem;
  --radius-md: calc(0.5rem - 2px);
  --radius-sm: calc(0.5rem - 4px);
}

/* Dark mode theme */
@media (prefers-color-scheme: dark) {
  :root {
    --color-background: oklch(0.09 0.005 285.82);
    --color-foreground: oklch(0.98 0.005 285.82);
    --color-primary: oklch(0.98 0.005 285.82);
    --color-primary-foreground: oklch(0.35 0.1 285.82);
    --color-secondary: oklch(0.20 0.02 285.82);
    --color-secondary-foreground: oklch(0.98 0.005 285.82);
    --color-muted: oklch(0.20 0.02 285.82);
    --color-muted-foreground: oklch(0.68 0.02 285.82);
    --color-accent: oklch(0.20 0.02 285.82);
    --color-accent-foreground: oklch(0.98 0.005 285.82);
    --color-destructive: oklch(0.38 0.15 29.23);
    --color-destructive-foreground: oklch(0.98 0.005 285.82);
    --color-border: oklch(0.20 0.02 285.82);
    --color-input: oklch(0.20 0.02 285.82);
    --color-ring: oklch(0.84 0.02 285.82);
  }
}

/* Class-based dark mode (for manual toggle) */
.dark {
  --color-background: oklch(0.09 0.005 285.82);
  --color-foreground: oklch(0.98 0.005 285.82);
  --color-primary: oklch(0.98 0.005 285.82);
  --color-primary-foreground: oklch(0.35 0.1 285.82);
  --color-secondary: oklch(0.20 0.02 285.82);
  --color-secondary-foreground: oklch(0.98 0.005 285.82);
  --color-muted: oklch(0.20 0.02 285.82);
  --color-muted-foreground: oklch(0.68 0.02 285.82);
  --color-accent: oklch(0.20 0.02 285.82);
  --color-accent-foreground: oklch(0.98 0.005 285.82);
  --color-destructive: oklch(0.38 0.15 29.23);
  --color-destructive-foreground: oklch(0.98 0.005 285.82);
  --color-border: oklch(0.20 0.02 285.82);
  --color-input: oklch(0.20 0.02 285.82);
  --color-ring: oklch(0.84 0.02 285.82);
}

/* Base layer styles */
@layer base {
  * {
    @apply border-border;
  }
  
  body {
    @apply bg-background text-foreground antialiased;
  }
} 
``` 

### Why OKLCH?

OKLCH provides better perceptual uniformity compared to HSL:
- More consistent lightness across hues
- Better interpolation between colors
- Easier to create accessible color palettes
- More accurate representation of human color perception

**Converting HSL to OKLCH:**
Use tools like [OKLCH Color Picker](https://oklch.com) to convert existing HSL values.

---

## Dark Mode Setup

### Vue 3 + Vite

**1. Install VueUse for theme management:**

```bash
npm install @vueuse/core
```

**2. Create theme composable:**

```typescript
// composables/useTheme.ts
import { useDark, useToggle, useStorage } from '@vueuse/core'

export function useTheme() {
  const isDark = useDark({
    storageKey: 'theme',
    valueDark: 'dark',
    valueLight: 'light',
  })
  
  const toggleTheme = useToggle(isDark)
  
  const theme = computed(() => isDark.value ? 'dark' : 'light')
  
  const setTheme = (newTheme: 'light' | 'dark' | 'system') => {
    if (newTheme === 'system') {
      const systemDark = window.matchMedia('(prefers-color-scheme: dark)').matches
      isDark.value = systemDark
    } else {
      isDark.value = newTheme === 'dark'
    }
  }
  
  return {
    isDark,
    theme,
    toggleTheme,
    setTheme,
  }
}
```

The complete content continues with all sections including Vue 3 setup, Inertia.js integration, Next.js setup, color customization, component customization, advanced patterns, accessibility, best practices, troubleshooting, and resources.