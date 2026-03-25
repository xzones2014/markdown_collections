# Tailwind CSS v4 Customization & Style Guide

## Overview

Tailwind v4 introduces a CSS-first configuration approach, moving away from the JavaScript-based `tailwind.config.js` file. This guide covers best practices for customizing your design system and building maintainable layouts.

---

## The @theme Directive

The `@theme` directive is the foundation of Tailwind v4 customization. Use it to define CSS variables that Tailwind automatically converts into utility classes.

```css
@import "tailwindcss";

@theme {
  /* Brand colors using OKLCH for perceptual uniformity */
  --color-brand-50: oklch(0.97 0.02 264);
  --color-brand-500: oklch(0.55 0.22 264);
  --color-brand-900: oklch(0.25 0.15 264);

  /* Typography */
  --font-display: "Satoshi", "Inter", sans-serif;
  --font-body: "Inter", system-ui, sans-serif;

  /* Spacing */
  --spacing-18: calc(var(--spacing) * 18);
  --spacing-navbar: 4.5rem;

  /* Custom breakpoints */
  --breakpoint-3xl: 120rem;
  --breakpoint-tablet: 48rem;

  /* Effects */
  --shadow-glow: 0 0 20px rgba(139, 92, 246, 0.3);
  --radius-large: 1.5rem;
}
```

---

## Layout Patterns & Best Practices

### 1. Form Layouts with CSS Grid

Use CSS Grid with `minmax()` to create flexible form layouts where labels maintain a minimum width while inputs fill available space.

**Recommended approach:**

```html
<div class="grid grid-cols-[minmax(0,_200px)_1fr] gap-2 items-center">
  <label class="font-medium text-slate-700">Username</label>
  <div>
    <input 
      type="text" 
      class="w-full rounded-md border-slate-300 shadow-sm focus:ring-brand-500" 
      required 
      autofocus 
    />
  </div>
</div>
```

**Benefits:**
- Labels remain readable at their natural width (up to 200px)
- Inputs responsively fill remaining space
- Consistent alignment across form fields

---

### 2. Container Queries

Container queries allow components to adapt based on their parent's size rather than the viewport, making them more portable and reusable.

```html
<div class="@container">
  <div class="grid grid-cols-1 @md:grid-cols-2 @lg:grid-cols-3 gap-4">
    <!-- Cards adapt to container width, not viewport width -->
    <div class="bg-white rounded-lg p-4">Card content</div>
  </div>
</div>
```

**When to use:**
- Sidebar components that may appear in different layouts
- Reusable card grids with varying container widths
- Dashboard widgets with dynamic positioning

---

### 3. Arbitrary Values with Square Brackets

For one-off styles, use arbitrary values directly in your markup instead of creating new utility classes.

```html
<!-- Custom gradient mask -->
<div class="[mask-image:linear-gradient(to_bottom,black,transparent)]">
  Fading content with inline arbitrary value
</div>

<!-- Custom grid template -->
<div class="grid grid-cols-[auto_1fr_auto] gap-4">
  <aside>Sidebar</aside>
  <main>Content</main>
  <aside>Widgets</aside>
</div>
```

**Guidelines:**
- Use for truly unique, non-reusable styles
- Prefer creating utilities for patterns used 3+ times
- Keep arbitrary values simple and readable

---

## Creating Custom Utilities

Use `@utility` to define reusable component styles that apply multiple properties.

```css
@utility glass {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

@utility shadow-glow {
  box-shadow: var(--shadow-glow);
}
```

**Usage in HTML:**

```html
<div class="glass rounded-lg p-6">
  Glassmorphism card effect
</div>
```

---

## Custom Variants

Define custom variants to target specific states, data attributes, or complex selectors.

```css
/* Theme-based styling */
@custom-variant theme-midnight (&:where([data-theme="midnight"] *));

/* ARIA state targeting */
@custom-variant aria-checked (&[aria-checked="true"]);

/* Group interactions */
@custom-variant group-active (.group:active &);
```

**Usage:**

```html
<div data-theme="midnight">
  <p class="theme-midnight:text-purple-300">Styled when midnight theme is active</p>
</div>

<button aria-checked="true" class="aria-checked:bg-brand-500">
  Toggle button
</button>
```

---

## CSS Layer Organization

Use `@layer` to organize styles and control specificity predictably.

```css
@layer base {
  body {
    @apply bg-slate-50 text-slate-900 antialiased;
  }
  
  h1, h2, h3 {
    @apply font-display font-bold tracking-tight;
  }
}

@layer components {
  .btn-primary {
    @apply inline-flex items-center justify-center px-4 py-2 
           bg-brand-500 text-white rounded-lg font-medium
           transition-transform hover:bg-brand-600 
           active:scale-95 focus:outline-none focus:ring-2 
           focus:ring-brand-500 focus:ring-offset-2;
  }
  
  .card {
    @apply bg-white rounded-xl shadow-sm border border-slate-200 p-6;
  }
}

@layer utilities {
  /* Custom utility overrides */
  .scroll-smooth {
    scroll-behavior: smooth;
  }
}
```

**Layer hierarchy (lowest to highest specificity):**
1. `base` - Element defaults and resets
2. `components` - Reusable component classes
3. `utilities` - Single-purpose utility classes

---

## Best Practices Checklist

✅ **Define design tokens in @theme first** - Establish your color palette, spacing scale, and typography before writing components

✅ **Use OKLCH for colors** - Provides better perceptual uniformity and easier lightness adjustments compared to HSL or HEX

✅ **Leverage CSS Grid for complex layouts** - Use arbitrary grid values like `grid-cols-[200px_1fr_auto]` for sidebars, forms, and app shells

✅ **Limit @apply usage** - Prefer utility classes in HTML for better performance and easier debugging. Reserve `@apply` for frequently reused component patterns

✅ **Embrace container queries** - Build components that adapt to their container for better reusability

✅ **Use layers strategically** - Organize CSS into `base`, `components`, and `utilities` layers to prevent specificity conflicts

✅ **Automatic content detection** - Tailwind v4 automatically scans your project files; no manual content configuration needed

---

## Common Patterns & Examples

### Responsive Sidebar Layout

```html
<div class="grid grid-cols-1 lg:grid-cols-[280px_1fr] gap-6">
  <aside class="bg-white rounded-lg p-4">
    <!-- Sidebar content -->
  </aside>
  <main>
    <!-- Main content -->
  </main>
</div>
```

### Glassmorphism Card

```html
<div class="glass rounded-xl p-8 backdrop-blur-lg">
  <h2 class="text-2xl font-display font-bold mb-4">Card Title</h2>
  <p class="text-slate-600">Beautiful glassmorphism effect</p>
</div>
```

### Data-Driven Theming

```html
<div data-theme="midnight" class="min-h-screen p-8">
  <h1 class="text-3xl theme-midnight:text-purple-200">
    Theme-aware heading
  </h1>
</div>
```

### Advanced Grid Form

```html
<form class="space-y-4">
  <div class="grid grid-cols-[minmax(0,_200px)_1fr] gap-2 items-center">
    <label class="font-medium text-slate-700">Full Name</label>
    <input type="text" class="w-full rounded-md border-slate-300 shadow-sm" />
  </div>
  
  <div class="grid grid-cols-[minmax(0,_200px)_1fr] gap-2 items-center">
    <label class="font-medium text-slate-700">Email Address</label>
    <input type="email" class="w-full rounded-md border-slate-300 shadow-sm" />
  </div>
  
  <div class="grid grid-cols-[minmax(0,_200px)_1fr] gap-2 items-start">
    <label class="font-medium text-slate-700 pt-2">Bio</label>
    <textarea rows="4" class="w-full rounded-md border-slate-300 shadow-sm"></textarea>
  </div>
</form>
```

---

## Additional Resources

- [Tailwind CSS v4 Official Documentation](https://tailwindcss.com/docs)
- [OKLCH Color Picker](https://oklch.com)
- [Container Queries Guide](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Container_Queries)
- [CSS Grid Layout Guide](https://css-tricks.com/snippets/css/complete-guide-grid/)

---

## Migration Tips from v3

### Before (v3):

```js
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        brand: {
          500: '#8b5cf6',
        }
      },
      fontFamily: {
        display: ['Satoshi', 'Inter', 'sans-serif'],
      }
    }
  },
  content: ['./src/**/*.{js,jsx,ts,tsx}'],
}
```

### After (v4):

```css
@import "tailwindcss";

@theme {
  --color-brand-500: oklch(0.55 0.22 264);
  --font-display: "Satoshi", "Inter", sans-serif;
}
```

**Key changes:**
- Replace `tailwind.config.js` theme extensions with `@theme` directive
- Convert custom plugins to `@utility` and `@custom-variant` directives
- Update color definitions to use OKLCH format for consistency
- Remove `content` configuration (handled automatically in v4)
- Move custom CSS into layers using `@layer` directive

---

## Troubleshooting

### Colors not appearing?

Make sure you're using the correct naming convention:
```css
@theme {
  --color-brand-500: oklch(0.55 0.22 264); /* ✅ Correct */
  --brand-500: oklch(0.55 0.22 264);       /* ❌ Won't work */
}
```

### Custom utilities not applying?

Ensure `@utility` is defined before your components:
```css
@import "tailwindcss";

@theme { /* ... */ }

@utility glass { /* ... */ }  /* Define utilities here */

@layer components { /* ... */ }
```

### Container queries not working?

Add the `@container` class to the parent element:
```html
<div class="@container">  <!-- ✅ Required -->
  <div class="@md:grid-cols-2">...</div>
</div>
```

---

## Performance Tips

1. **Avoid over-nesting @apply** - Each `@apply` adds to bundle size
2. **Use arbitrary values sparingly** - They can't be purged as efficiently
3. **Leverage CSS variables** - Define once in `@theme`, use everywhere
4. **Minimize custom utilities** - Use built-in Tailwind utilities when possible
5. **Enable JIT mode** - Enabled by default in v4 for optimal performance

---

## Conclusion

Tailwind CSS v4's CSS-first approach simplifies configuration while providing more flexibility. By mastering `@theme`, custom utilities, variants, and modern CSS features like container queries and CSS Grid, you can build maintainable, performant, and beautiful user interfaces.

For questions or contributions, refer to the [official Tailwind CSS community](https://github.com/tailwindlabs/tailwindcss/discussions).