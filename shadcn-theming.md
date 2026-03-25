# Comprehensive shadcn/ui Theming Guide

## Introduction
This guide provides detailed instructions on how to implement theming using shadcn/ui with Tailwind CSS v4, Vue 3, and Inertia.js, utilizing OKLCH colors for improved color management and offering configurations for dark mode. It also covers component customization and best practices to maximize the use of these technologies together.

## Getting Started
To begin, ensure you have the latest versions of the required packages installed:

```bash
bun add tailwindcss@latest vue@next inertiajs/inertia-vue3
```

## Setting Up Tailwind CSS v4
Tailwind CSS helps in designing your UI with utility-first styling. Follow these steps to integrate it with your Vue project:

1. Create your `tailwind.config.js` file:
   ```javascript
   module.exports = {
     content: ["./src/**/*.{vue,js,ts,jsx,tsx}", "./public/index.html"],
     theme: {
       extend: {
         colors: {
           // Define your custom OKLCH colors here
         },
       },
     },
   };
   ```

2. Import Tailwind in your main CSS file:
   ```css
   @tailwind base;
   @tailwind components;
   @tailwind utilities;
   ```

## OKLCH Color Implementation
Utilize OKLCH colors for your components. Define your palette in the Tailwind configuration to achieve consistency across your application.

## Dark Mode Setup
To set up dark mode, you can use the `media` strategy:

```javascript
tailwind.config.js:
module.exports = {
  darkMode: "media",
};
```

## Component Customization
With shadcn/ui components, you can rent or style them based on your requirements. Explore the available props and customize them:

For example, adding a custom class:
```vue
<SomeComponent class="bg-blue-500" />
```

## Best Practices
- Always keep your dependencies updated with:
```bash
bun add package-name
```
- Avoid using inline styles for maintainability.
- Organize your theme styles for easy modifications.

## Conclusion
By following this guide, you’ll be equipped to effectively use shadcn/ui with Tailwind CSS v4, ensuring your application is visually appealing and consistent across different themes and modes.