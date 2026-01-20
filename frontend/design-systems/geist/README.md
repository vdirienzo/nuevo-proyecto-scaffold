# Geist Design System

> Vercel/Geist aesthetic design system with pure black backgrounds, high contrast, and subtle blue-violet-magenta gradients.

**Autor:** {{AUTHOR}}
**Project:** {{PROJECT_NAME}}

---

## Overview

The Geist Design System provides production-ready components with the following features:

- **Pure black backgrounds** (#000, #0a0a0a)
- **High contrast** white/light gray text
- **Subtle gradients** blue-violet-magenta
- **Ultra-clean typography** (Geist Sans / Inter)
- **Multi-layer soft shadows** for depth
- **Generous spacing** and sharp minimalism
- **Smooth animations** and transitions
- **React + Vue 3** component libraries
- **TypeScript** support
- **Fully customizable** with Tailwind CSS

---

## Installation

### 1. Install Tailwind CSS

If you haven't already, install Tailwind CSS in your project:

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

### 2. Copy Configuration Files

Copy the Geist design system files to your project:

```bash
# Copy Tailwind config
cp tailwind.config.geist.js.tmpl tailwind.config.js

# Copy global styles
cp globals.css.tmpl src/styles/globals.css

# Copy components (React or Vue)
cp -r components/react src/components/geist  # For React
# or
cp -r components/vue src/components/geist    # For Vue
```

### 3. Import Global Styles

**Next.js / React:**

```tsx
// app/layout.tsx or pages/_app.tsx
import '@/styles/globals.css'
```

**Vue 3:**

```typescript
// main.ts
import './styles/globals.css'
```

### 4. Install Additional Dependencies

```bash
# For React components
npm install class-variance-authority clsx tailwind-merge

# For Vue components (already included in Vue 3)
# No additional dependencies needed
```

---

## Component Library

### React Components

- [Button](#button) - Primary, secondary, outline, ghost, danger, glow variants
- [Card](#card) - Default, elevated, glass, gradient variants
- [Input](#input) - Text input with label, error states, and icons
- [Modal](#modal) - Centered modal with gradient border accent
- [Navbar](#navbar) - Responsive navigation with blur backdrop

### Vue Components

- [Button (Vue)](#button-vue) - All button variants
- [Card (Vue)](#card-vue) - All card variants
- [Input (Vue)](#input-vue) - Input with v-model support

---

## Usage Examples

### Button

#### React

```tsx
import { Button } from '@/components/geist/Button'

export default function Example() {
  return (
    <>
      {/* Primary gradient button */}
      <Button variant="primary" size="md">
        Get Started
      </Button>

      {/* Secondary with icon */}
      <Button
        variant="secondary"
        leftIcon={<Icon />}
      >
        Learn More
      </Button>

      {/* Loading state */}
      <Button loading>
        Processing...
      </Button>

      {/* Glow effect */}
      <Button variant="glow" size="lg">
        Launch Now
      </Button>
    </>
  )
}
```

#### Vue

```vue
<template>
  <div>
    <!-- Primary gradient button -->
    <Button variant="primary" size="md">
      Get Started
    </Button>

    <!-- Secondary with icon -->
    <Button variant="secondary">
      <template #leftIcon>
        <Icon />
      </template>
      Learn More
    </Button>

    <!-- Loading state -->
    <Button :loading="true">
      Processing...
    </Button>

    <!-- Glow effect -->
    <Button variant="glow" size="lg">
      Launch Now
    </Button>
  </div>
</template>

<script setup lang="ts">
import Button from '@/components/geist/Button.vue'
</script>
```

**Available Props:**

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `variant` | `'primary' \| 'secondary' \| 'outline' \| 'ghost' \| 'danger' \| 'glow'` | `'primary'` | Button style variant |
| `size` | `'sm' \| 'md' \| 'lg' \| 'icon'` | `'md'` | Button size |
| `loading` | `boolean` | `false` | Show loading spinner |
| `disabled` | `boolean` | `false` | Disable button |
| `leftIcon` | `ReactNode` (React) or slot (Vue) | - | Icon before text |
| `rightIcon` | `ReactNode` (React) or slot (Vue) | - | Icon after text |

---

### Card

#### React

```tsx
import {
  Card,
  CardHeader,
  CardTitle,
  CardDescription,
  CardContent,
  CardFooter
} from '@/components/geist/Card'

export default function Example() {
  return (
    <>
      {/* Basic card */}
      <Card>
        <h3>Card Title</h3>
        <p>Card content goes here.</p>
      </Card>

      {/* Card with header and footer */}
      <Card
        variant="gradient"
        hover="lift"
      >
        <CardHeader>
          <CardTitle>Premium Feature</CardTitle>
          <CardDescription>
            This is a premium feature description
          </CardDescription>
        </CardHeader>
        <CardContent>
          <p>Main content area</p>
        </CardContent>
        <CardFooter>
          <Button>Learn More</Button>
        </CardFooter>
      </Card>

      {/* Glass card */}
      <Card variant="glass" padding="lg">
        Glassmorphism effect with backdrop blur
      </Card>
    </>
  )
}
```

#### Vue

```vue
<template>
  <div>
    <!-- Basic card -->
    <Card>
      <h3>Card Title</h3>
      <p>Card content goes here.</p>
    </Card>

    <!-- Card with header and footer -->
    <Card variant="gradient" hover="lift">
      <template #header>
        <h3 class="text-xl font-semibold">Premium Feature</h3>
        <p class="text-sm text-geist-gray-400">
          This is a premium feature description
        </p>
      </template>

      <p>Main content area</p>

      <template #footer>
        <Button>Learn More</Button>
      </template>
    </Card>

    <!-- Glass card -->
    <Card variant="glass" padding="lg">
      Glassmorphism effect with backdrop blur
    </Card>
  </div>
</template>

<script setup lang="ts">
import Card from '@/components/geist/Card.vue'
import Button from '@/components/geist/Button.vue'
</script>
```

**Available Props:**

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `variant` | `'default' \| 'elevated' \| 'glass' \| 'gradient'` | `'default'` | Card style variant |
| `hover` | `'none' \| 'lift' \| 'glow' \| 'scale'` | `'glow'` | Hover effect |
| `padding` | `'none' \| 'sm' \| 'md' \| 'lg'` | `'md'` | Internal padding |

---

### Input

#### React

```tsx
import { Input, Textarea } from '@/components/geist/Input'

export default function Example() {
  return (
    <>
      {/* Basic input */}
      <Input
        label="Email"
        placeholder="you@example.com"
        type="email"
        required
      />

      {/* Input with icons */}
      <Input
        label="Search"
        placeholder="Search..."
        leftIcon={<SearchIcon />}
        rightIcon={<ClearIcon />}
      />

      {/* Input with error */}
      <Input
        label="Password"
        type="password"
        error="Password must be at least 8 characters"
      />

      {/* Gradient focus variant */}
      <Input
        variant="gradient"
        label="Username"
        helperText="This will be your public username"
      />

      {/* Textarea */}
      <Textarea
        label="Message"
        placeholder="Enter your message..."
        rows={4}
      />
    </>
  )
}
```

#### Vue

```vue
<template>
  <div>
    <!-- Basic input -->
    <Input
      v-model="email"
      label="Email"
      placeholder="you@example.com"
      type="email"
      :required="true"
    />

    <!-- Input with icons -->
    <Input
      v-model="search"
      label="Search"
      placeholder="Search..."
    >
      <template #leftIcon>
        <SearchIcon />
      </template>
      <template #rightIcon>
        <ClearIcon />
      </template>
    </Input>

    <!-- Input with error -->
    <Input
      v-model="password"
      label="Password"
      type="password"
      error="Password must be at least 8 characters"
    />

    <!-- Gradient focus variant -->
    <Input
      v-model="username"
      variant="gradient"
      label="Username"
      helper-text="This will be your public username"
    />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import Input from '@/components/geist/Input.vue'

const email = ref('')
const search = ref('')
const password = ref('')
const username = ref('')
</script>
```

**Available Props:**

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `label` | `string` | - | Input label |
| `placeholder` | `string` | - | Placeholder text |
| `helperText` | `string` | - | Helper text below input |
| `error` | `string` | - | Error message (overrides helperText) |
| `variant` | `'default' \| 'gradient' \| 'error'` | `'default'` | Input style variant |
| `size` | `'sm' \| 'md' \| 'lg'` | `'md'` | Input size |
| `leftIcon` | `ReactNode` (React) or slot (Vue) | - | Icon before input |
| `rightIcon` | `ReactNode` (React) or slot (Vue) | - | Icon after input |

---

### Modal

#### React

```tsx
import { useState } from 'react'
import { Modal } from '@/components/geist/Modal'
import { Button } from '@/components/geist/Button'

export default function Example() {
  const [open, setOpen] = useState(false)

  return (
    <>
      <Button onClick={() => setOpen(true)}>
        Open Modal
      </Button>

      <Modal
        open={open}
        onClose={() => setOpen(false)}
        title="Confirm Action"
        description="Are you sure you want to proceed?"
        size="md"
        footer={
          <>
            <Button variant="ghost" onClick={() => setOpen(false)}>
              Cancel
            </Button>
            <Button variant="primary" onClick={() => setOpen(false)}>
              Confirm
            </Button>
          </>
        }
      >
        <p>This action cannot be undone.</p>
      </Modal>
    </>
  )
}
```

**Available Props:**

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `open` | `boolean` | `false` | Controls modal visibility |
| `onClose` | `() => void` | - | Callback when modal should close |
| `title` | `string` | - | Modal title |
| `description` | `string` | - | Modal description |
| `size` | `'sm' \| 'md' \| 'lg' \| 'xl' \| 'full'` | `'md'` | Modal size |
| `footer` | `ReactNode` | - | Footer content (buttons) |
| `disableBackdropClick` | `boolean` | `false` | Disable closing on backdrop click |
| `disableEscapeKey` | `boolean` | `false` | Disable closing on Escape key |
| `showCloseButton` | `boolean` | `true` | Show close button in header |

---

### Navbar

#### React

```tsx
import { Navbar } from '@/components/geist/Navbar'
import { Button } from '@/components/geist/Button'

export default function Example() {
  const links = [
    { label: 'Home', href: '/', active: true },
    { label: 'Features', href: '/features' },
    { label: 'Pricing', href: '/pricing' },
    { label: 'Docs', href: '/docs' },
  ]

  return (
    <Navbar
      logo="{{PROJECT_NAME}}"
      links={links}
      rightContent={
        <>
          <Button variant="ghost" size="sm">
            Sign In
          </Button>
          <Button variant="primary" size="sm">
            Get Started
          </Button>
        </>
      }
    />
  )
}
```

**Available Props:**

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `logo` | `ReactNode` | `'{{PROJECT_NAME}}'` | Logo component or text |
| `links` | `NavLink[]` | `[]` | Navigation links |
| `rightContent` | `ReactNode` | - | Right-side content (CTA, auth) |
| `sticky` | `boolean` | `true` | Sticky positioning |

**NavLink Interface:**

```typescript
interface NavLink {
  label: string
  href: string
  active?: boolean
}
```

---

## Utility Classes

The design system includes custom utility classes for common patterns:

### Gradient Text

```html
<h1 class="text-gradient">
  Gradient Text
</h1>

<h2 class="text-gradient-blue">
  Blue Gradient
</h2>

<h3 class="text-gradient-violet">
  Violet Gradient
</h3>
```

### Gradient Backgrounds

```html
<div class="bg-gradient-primary">
  Primary gradient background
</div>

<div class="bg-gradient-subtle">
  Subtle gradient background
</div>
```

### Border Gradient

```html
<div class="border-gradient p-6">
  Card with gradient border
</div>
```

### Glassmorphism

```html
<div class="glass p-6">
  Glass effect with backdrop blur
</div>

<div class="glass-strong p-6">
  Stronger glass effect
</div>
```

### Glow Effects

```html
<div class="glow-blue p-6">
  Blue glow shadow
</div>

<div class="glow-violet p-6">
  Violet glow shadow
</div>

<div class="glow-magenta p-6">
  Magenta glow shadow
</div>
```

### Shimmer Effect

```html
<div class="shimmer p-6">
  Animated shimmer effect
</div>
```

### Custom Scrollbar

```html
<div class="scrollbar-geist h-64 overflow-y-auto">
  Content with custom scrollbar
</div>

<!-- Hide scrollbar -->
<div class="scrollbar-none h-64 overflow-y-auto">
  Content with hidden scrollbar
</div>
```

---

## Color Palette

### Base Colors

```css
--geist-black: #000000
--geist-black-lighter: #0a0a0a
--geist-black-card: #111111
--geist-white: #ffffff
```

### Gray Scale

```css
--geist-gray-50: #fafafa
--geist-gray-100: #f4f4f5
--geist-gray-200: #e4e4e7
--geist-gray-300: #d4d4d8
--geist-gray-400: #a1a1aa
--geist-gray-500: #71717a
--geist-gray-600: #52525b
--geist-gray-700: #3f3f46
--geist-gray-800: #27272a
--geist-gray-900: #18181b
--geist-gray-950: #0a0a0a
```

### Accent Colors

```css
--geist-blue: #0070f3
--geist-violet: #7928ca
--geist-magenta: #ff0080
--geist-cyan: #50e3c2
```

### Status Colors

```css
--geist-success: #0070f3
--geist-error: #ee0000
--geist-warning: #f5a623
```

---

## Typography

### Font Families

```css
--font-geist-sans: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif
--font-geist-mono: 'Menlo', Monaco, 'Courier New', monospace
```

### Font Classes

```html
<p class="font-geist">Geist Sans font</p>
<code class="font-geist-mono">Geist Mono font</code>
```

---

## Customization

### Tailwind Config

The `tailwind.config.geist.js` file contains all design tokens. Customize colors, spacing, shadows, etc.:

```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        'geist-blue': {
          DEFAULT: '#0070f3',
          light: '#3291ff',
          dark: '#0761d1',
        },
        // Add your custom colors
      },
    },
  },
}
```

### CSS Variables

Override CSS variables in your `globals.css`:

```css
:root {
  --geist-blue: #your-color;
  --spacing-lg: 2rem;
  /* etc. */
}
```

---

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

---

## License

This design system is part of {{PROJECT_NAME}}.

**Autor:** {{AUTHOR}}

---

## Changelog

### Version 1.0.0 (Initial Release)

#### Added
- Complete Tailwind CSS configuration with Geist aesthetic
- Global styles with CSS variables and utility classes
- React components: Button, Card, Input, Modal, Navbar
- Vue 3 components: Button, Card, Input
- TypeScript support for all components
- Production-ready component variants
- Comprehensive documentation with examples

---

## Resources

- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [Vercel Design](https://vercel.com/design)
- [React Documentation](https://react.dev)
- [Vue 3 Documentation](https://vuejs.org)
