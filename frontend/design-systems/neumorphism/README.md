# Neumorphism (Soft UI) Design System

A complete neumorphism design system featuring soft, tactile interfaces with inset and outset shadows for a calm, raised/pressed aesthetic.

**Author:** {{AUTHOR}}
**Project:** {{PROJECT_NAME}}

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Installation](#installation)
- [Design Principles](#design-principles)
- [Components](#components)
  - [React](#react-components)
  - [Vue 3](#vue-3-components)
- [Styling](#styling)
- [Theme Configuration](#theme-configuration)
- [Best Practices](#best-practices)
- [Accessibility](#accessibility)
- [Browser Support](#browser-support)
- [Examples](#examples)

---

## Overview

Neumorphism (also known as "Soft UI") is a design trend that creates a soft, extruded plastic look using subtle shadows. This design system provides:

- **React** and **Vue 3** components
- **Tailwind CSS** configuration with custom shadows
- **Light and dark mode** support
- **Fully accessible** components
- **Type-safe** with TypeScript

---

## Features

- ✅ **7 React Components**: Button, Card, Input, Toggle, Slider, Checkbox, Progress
- ✅ **4 Vue 3 Components**: Button, Card, Input, Toggle
- ✅ **Custom Tailwind Config**: Pre-configured shadows and utilities
- ✅ **Dark Mode**: Automatic theme switching
- ✅ **Animations**: Smooth transitions and press effects
- ✅ **Accessible**: WCAG 2.1 AA compliant
- ✅ **Type-Safe**: Full TypeScript support

---

## Installation

### 1. Copy Templates

Copy the design system files to your project:

```bash
cp -r neumorphism/ /path/to/your/project/
```

### 2. Install Dependencies

```bash
# For React projects
npm install clsx tailwind-merge
# or
yarn add clsx tailwind-merge
# or
pnpm add clsx tailwind-merge

# For Vue projects (already included in Vue 3)
```

### 3. Update Tailwind Config

Merge `tailwind.config.neu.js.tmpl` with your existing `tailwind.config.js`:

```js
// tailwind.config.js
module.exports = {
  // ... existing config
  extend: {
    // Copy the theme extensions from tailwind.config.neu.js.tmpl
  },
  plugins: [
    // Copy the plugins from tailwind.config.neu.js.tmpl
  ],
};
```

### 4. Import Global Styles

```tsx
// In your main entry file (e.g., app.tsx, main.tsx, or _app.tsx)
import './neumorphism/globals.css';
```

### 5. Create Utility Helper (React only)

Create `lib/utils.ts`:

```ts
import { clsx, type ClassValue } from 'clsx';
import { twMerge } from 'tailwind-merge';

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

---

## Design Principles

### Shadow Formula

Neumorphism relies on dual shadows (light and dark) to create depth:

**Light Mode:**
- Outset (raised): `8px 8px 16px #b8bec7, -8px -8px 16px #ffffff`
- Inset (pressed): `inset 8px 8px 16px #b8bec7, inset -8px -8px 16px #ffffff`

**Dark Mode:**
- Outset (raised): `8px 8px 16px #1a1a1a, -8px -8px 16px #404040`
- Inset (pressed): `inset 8px 8px 16px #1a1a1a, inset -8px -8px 16px #404040`

### Visual Effects

| Effect | Usage | Shadow Type |
|--------|-------|-------------|
| **Flat** | Standard surface | Medium outset |
| **Raised** | Primary actions | Large outset |
| **Convex** | Subtle elevation | Small outset |
| **Pressed** | Active/focused inputs | Inset |
| **Concave** | Subtle inset | Small inset |

### Color Palette

- **Background**: Same as component surface for seamless blending
- **Shadows**: Very subtle contrast (10-15% difference)
- **Text**: Low contrast for calm aesthetic
- **Accents**: Use sparingly for important actions

---

## Components

### React Components

All React components support:
- TypeScript props with full type safety
- `forwardRef` for ref forwarding
- Accessibility attributes (ARIA)
- Size variants (`sm`, `md`, `lg`)
- Disabled and loading states

#### NeuButton

```tsx
import NeuButton from './components/react/NeuButton';

<NeuButton variant="raised" size="md" onClick={handleClick}>
  Click me
</NeuButton>
```

**Props:**
- `variant`: `'raised' | 'flat' | 'pressed'`
- `size`: `'sm' | 'md' | 'lg'`
- `fullWidth`: `boolean`
- `loading`: `boolean`
- `iconOnly`: `boolean`

#### NeuCard

```tsx
import NeuCard from './components/react/NeuCard';

<NeuCard variant="flat" hoverable padding="md">
  <h3>Card Title</h3>
  <p>Card content</p>
</NeuCard>
```

**Props:**
- `variant`: `'flat' | 'convex' | 'raised' | 'pressed'`
- `hoverable`: `boolean`
- `padding`: `'none' | 'sm' | 'md' | 'lg'`
- `rounded`: `'sm' | 'md' | 'lg' | 'xl'`

#### NeuInput

```tsx
import NeuInput from './components/react/NeuInput';

<NeuInput
  label="Email"
  floatingLabel
  placeholder=" "
  leftIcon={<EmailIcon />}
/>
```

**Props:**
- `label`: `string`
- `floatingLabel`: `boolean`
- `size`: `'sm' | 'md' | 'lg'`
- `error`: `string`
- `helperText`: `string`
- `leftIcon`: `ReactNode`
- `rightIcon`: `ReactNode`

#### NeuToggle

```tsx
import NeuToggle from './components/react/NeuToggle';

<NeuToggle
  checked={enabled}
  onCheckedChange={setEnabled}
  label="Dark mode"
/>
```

**Props:**
- `checked`: `boolean`
- `onCheckedChange`: `(checked: boolean) => void`
- `label`: `string`
- `labelPosition`: `'left' | 'right'`
- `size`: `'sm' | 'md' | 'lg'`

#### NeuSlider

```tsx
import NeuSlider from './components/react/NeuSlider';

<NeuSlider
  value={volume}
  onValueChange={setVolume}
  min={0}
  max={100}
  label="Volume"
/>
```

**Props:**
- `value`: `number`
- `onValueChange`: `(value: number) => void`
- `min`: `number`
- `max`: `number`
- `step`: `number`
- `showValue`: `boolean`
- `formatValue`: `(value: number) => string`

#### NeuCheckbox

```tsx
import NeuCheckbox from './components/react/NeuCheckbox';

<NeuCheckbox
  checked={accepted}
  onCheckedChange={setAccepted}
  label="I agree to terms"
/>
```

**Props:**
- `checked`: `boolean`
- `onCheckedChange`: `(checked: boolean) => void`
- `label`: `string`
- `labelPosition`: `'left' | 'right'`
- `indeterminate`: `boolean`
- `size`: `'sm' | 'md' | 'lg'`

#### NeuProgress

```tsx
import NeuProgress from './components/react/NeuProgress';

<NeuProgress
  value={75}
  label="Upload Progress"
  showLabel
  gradient
/>
```

**Props:**
- `value`: `number` (0-100)
- `label`: `string`
- `showLabel`: `boolean`
- `formatLabel`: `(value: number) => string`
- `gradient`: `boolean`
- `animated`: `boolean`
- `indeterminate`: `boolean`

---

### Vue 3 Components

All Vue components support:
- `<script setup>` with TypeScript
- `v-model` binding
- Slots for custom content
- Composition API with `computed` refs

#### NeuButton (Vue)

```vue
<script setup>
import NeuButton from './components/vue/NeuButton.vue';
</script>

<template>
  <NeuButton variant="raised" @click="handleClick">
    Click me
  </NeuButton>
</template>
```

#### NeuCard (Vue)

```vue
<NeuCard variant="flat" :hoverable="true">
  <h3>Card Title</h3>
  <p>Card content</p>
</NeuCard>
```

#### NeuInput (Vue)

```vue
<NeuInput
  v-model="email"
  label="Email"
  :floating-label="true"
  placeholder=" "
>
  <template #left-icon>
    <EmailIcon />
  </template>
</NeuInput>
```

#### NeuToggle (Vue)

```vue
<NeuToggle
  v-model="darkMode"
  label="Dark mode"
  label-position="left"
/>
```

---

## Styling

### CSS Variables

All neumorphism styles use CSS custom properties for easy theming:

```css
:root {
  --neu-bg: 224 229 236; /* Light background */
  --neu-shadow-dark: 163 177 198;
  --neu-shadow-light: 255 255 255;
  --neu-text-primary: 26 26 26;
  --neu-accent: 99 102 241;
  /* ... more variables */
}

.dark {
  --neu-bg: 45 45 45; /* Dark background */
  --neu-shadow-dark: 26 26 26;
  --neu-shadow-light: 64 64 64;
  /* ... overrides for dark mode */
}
```

### Utility Classes

Pre-built classes for common patterns:

```html
<!-- Surface with standard shadow -->
<div class="neu-surface">...</div>

<!-- Pressed/inset effect -->
<div class="neu-pressed">...</div>

<!-- Raised effect -->
<div class="neu-raised">...</div>

<!-- Convex (subtle raise) -->
<div class="neu-convex">...</div>

<!-- Text colors -->
<p class="neu-text">Primary text</p>
<p class="neu-text-secondary">Secondary text</p>
<p class="neu-text-muted">Muted text</p>
```

---

## Theme Configuration

### Dark Mode Toggle

```tsx
// React example
import { useEffect, useState } from 'react';

export function ThemeToggle() {
  const [dark, setDark] = useState(false);

  useEffect(() => {
    if (dark) {
      document.documentElement.classList.add('dark');
    } else {
      document.documentElement.classList.remove('dark');
    }
  }, [dark]);

  return (
    <NeuToggle
      checked={dark}
      onCheckedChange={setDark}
      label="Dark mode"
    />
  );
}
```

```vue
<!-- Vue example -->
<script setup>
import { ref, watch } from 'vue';
import NeuToggle from './components/vue/NeuToggle.vue';

const darkMode = ref(false);

watch(darkMode, (isDark) => {
  if (isDark) {
    document.documentElement.classList.add('dark');
  } else {
    document.documentElement.classList.remove('dark');
  }
});
</script>

<template>
  <NeuToggle v-model="darkMode" label="Dark mode" />
</template>
```

### Custom Colors

Override CSS variables in your root styles:

```css
:root {
  --neu-accent: 239 68 68; /* Red accent */
  --neu-accent-shadow: 220 38 38;
}
```

---

## Best Practices

### ✅ DO

- Use neumorphism for **tactile interfaces** (dashboards, settings panels)
- Keep **contrast low** for the signature soft look
- Use **accent colors sparingly** for important actions
- Ensure **adequate spacing** between elements
- Apply **rounded corners** (`rounded-soft-*`)
- Test in **both light and dark modes**

### ❌ DON'T

- Don't use on **text-heavy content** (readability issues)
- Avoid **high contrast** combinations (breaks aesthetic)
- Don't stack too many **raised elements** (loses depth perception)
- Avoid on **small screens** (shadows can overwhelm)
- Don't use for **complex data visualizations**

---

## Accessibility

All components follow WCAG 2.1 AA guidelines:

- ✅ **Keyboard navigation**: Full support with `Tab`, `Enter`, `Space`
- ✅ **Screen readers**: Proper ARIA labels and roles
- ✅ **Focus indicators**: Custom neumorphic focus rings
- ✅ **Color contrast**: Tested for readability (though inherently low contrast)
- ✅ **Motion**: Respects `prefers-reduced-motion`

**Note:** Neumorphism's low contrast can be challenging for visually impaired users. Consider providing a high-contrast mode or alternative theme.

---

## Browser Support

- ✅ Chrome 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Edge 90+
- ⚠️ IE 11: Not supported (CSS custom properties required)

**CSS Features Used:**
- CSS Custom Properties (variables)
- `box-shadow` with multiple shadows
- `backdrop-filter` (for glass effect)
- `@supports` queries

---

## Examples

### Login Form

```tsx
<NeuCard variant="flat" padding="lg" rounded="xl">
  <h2 className="text-2xl font-bold mb-6 neu-text">Welcome Back</h2>

  <NeuInput
    label="Email"
    floatingLabel
    type="email"
    placeholder=" "
    className="mb-4"
  />

  <NeuInput
    label="Password"
    floatingLabel
    type="password"
    placeholder=" "
    className="mb-4"
  />

  <div className="flex items-center justify-between mb-6">
    <NeuCheckbox label="Remember me" />
    <a href="#" className="text-sm neu-accent">Forgot password?</a>
  </div>

  <NeuButton variant="raised" fullWidth>
    Sign In
  </NeuButton>
</NeuCard>
```

### Settings Panel

```tsx
<NeuCard variant="convex" padding="lg">
  <h3 className="text-lg font-semibold mb-4 neu-text">Settings</h3>

  <div className="space-y-4">
    <NeuToggle label="Dark mode" labelPosition="left" />
    <NeuToggle label="Notifications" labelPosition="left" />
    <NeuToggle label="Auto-save" labelPosition="left" />
  </div>

  <div className="mt-6">
    <NeuSlider
      label="Volume"
      min={0}
      max={100}
      showValue
      formatValue={(val) => `${val}%`}
    />
  </div>
</NeuCard>
```

### Dashboard Widget

```tsx
<NeuCard variant="raised" hoverable padding="lg" rounded="lg">
  <div className="flex items-center justify-between mb-4">
    <h4 className="font-semibold neu-text">Project Progress</h4>
    <span className="text-sm neu-text-muted">75% Complete</span>
  </div>

  <NeuProgress
    value={75}
    gradient
    animated
    size="md"
  />

  <div className="mt-4 flex gap-2">
    <NeuButton size="sm" variant="flat">Details</NeuButton>
    <NeuButton size="sm" variant="flat">Share</NeuButton>
  </div>
</NeuCard>
```

---

## Changelog

### v1.0.0 - Initial Release

**Added:**
- React components: Button, Card, Input, Toggle, Slider, Checkbox, Progress
- Vue 3 components: Button, Card, Input, Toggle
- Tailwind CSS configuration with neumorphic shadows
- Global CSS with theme variables
- Dark mode support
- TypeScript definitions
- Accessibility features
- Complete documentation

---

## Author

**{{AUTHOR}}**

---

## License

This design system is part of the **{{PROJECT_NAME}}** scaffold.

---

## Resources

- [Neumorphism.io](https://neumorphism.io) - Shadow generator
- [Tailwind CSS Docs](https://tailwindcss.com)
- [React Docs](https://react.dev)
- [Vue 3 Docs](https://vuejs.org)
- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)

---

**Happy building with Neumorphism!** ✨
