# Glassmorphism Design System

> Futuristic glass-like UI components with transparency, blur effects, and glowing borders

**Autor:** {{AUTHOR}}
**Proyecto:** {{PROJECT_NAME}}

---

## Overview

Este sistema de diseño implementa el estilo **Glassmorphism** (morfismo de vidrio), caracterizado por:

- ✨ **Transparencia + Backdrop Blur** - Efecto de cristal esmerilado
- 🌟 **Glowing Borders** - Bordes con resplandor sutil
- 🎨 **Vibrant Colors** - Colores vibrantes sobre fondo oscuro
- 🏔️ **Depth & Layers** - Profundidad visual mediante capas
- 🚀 **Futuristic Feel** - Estética moderna y futurista

---

## Installation

### 1. Install Dependencies

```bash
npm install tailwindcss autoprefixer postcss
npm install clsx tailwind-merge  # Para cn() utility
```

### 2. Setup Tailwind Config

Reemplaza tu `tailwind.config.js` con el archivo `tailwind.config.glass.js.tmpl`:

```js
// tailwind.config.js
module.exports = {
  content: ['./src/**/*.{js,jsx,ts,tsx,vue}'],
  theme: {
    extend: {
      // ... configuración de glassmorphism
    },
  },
}
```

### 3. Import Global Styles

Importa `globals.css` en tu aplicación:

```tsx
// React/Next.js - app/layout.tsx o pages/_app.tsx
import './globals.css'

// Vue - main.ts
import './globals.css'
```

### 4. Setup Utility Function

Crea un helper para combinar clases (si no existe):

```ts
// lib/utils.ts
import { type ClassValue, clsx } from 'clsx'
import { twMerge } from 'tailwind-merge'

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}
```

---

## Components

### React Components

#### GlassCard

```tsx
import { GlassCard } from '@/components/GlassCard'

<GlassCard variant="light" intensity="lg" hover glow>
  <h3 className="text-xl font-bold mb-2">Title</h3>
  <p className="text-white/80">Content goes here...</p>
</GlassCard>
```

**Props:**
- `variant`: `'light' | 'dark' | 'purple' | 'blue' | 'cyan' | 'pink'` (default: `'light'`)
- `intensity`: `'sm' | 'md' | 'lg'` - Blur intensity (default: `'lg'`)
- `hover`: `boolean` - Enable hover effects (default: `true`)
- `glow`: `boolean` - Enable glow shadow (default: `false`)

---

#### GlassButton

```tsx
import { GlassButton } from '@/components/GlassButton'

<GlassButton variant="primary" size="md" loading={false}>
  Click Me
</GlassButton>
```

**Props:**
- `variant`: `'primary' | 'secondary' | 'ghost'` (default: `'primary'`)
- `size`: `'sm' | 'md' | 'lg'` (default: `'md'`)
- `loading`: `boolean` - Show loading spinner (default: `false`)
- `ripple`: `boolean` - Enable ripple effect (default: `true`)

**Features:**
- Ripple effect on click
- Loading state with spinner
- Glowing border on hover
- Accessible keyboard navigation

---

#### GlassInput

```tsx
import { GlassInput } from '@/components/GlassInput'

<GlassInput
  label="Email"
  type="email"
  error={errors.email}
  helperText="Enter your email address"
/>
```

**Props:**
- `label`: `string` - Floating label text
- `error`: `string` - Error message (shows red glow)
- `helperText`: `string` - Helper text below input

**Features:**
- Floating label animation
- Focus glow effect
- Error state with red glow
- Accessible ARIA labels

---

#### GlassModal

```tsx
import { GlassModal } from '@/components/GlassModal'

const [isOpen, setIsOpen] = useState(false)

<GlassModal
  isOpen={isOpen}
  onClose={() => setIsOpen(false)}
  title="Modal Title"
  size="md"
>
  <p>Modal content...</p>
</GlassModal>
```

**Props:**
- `isOpen`: `boolean` - Control modal visibility
- `onClose`: `() => void` - Close callback
- `title`: `string` - Modal title (optional)
- `size`: `'sm' | 'md' | 'lg' | 'xl'` (default: `'md'`)
- `showCloseButton`: `boolean` (default: `true`)

**Features:**
- Heavy backdrop blur
- ESC key to close
- Click outside to close
- Scale + fade animation
- Body scroll lock

---

#### GlassNavbar

```tsx
import { GlassNavbar } from '@/components/GlassNavbar'

const navItems = [
  { label: 'Home', href: '/' },
  {
    label: 'Products',
    href: '/products',
    children: [
      { label: 'Category 1', href: '/products/cat1' },
      { label: 'Category 2', href: '/products/cat2' },
    ]
  },
  { label: 'About', href: '/about' },
]

<GlassNavbar logo={<Logo />} items={navItems} />
```

**Props:**
- `logo`: `React.ReactNode` - Logo component
- `items`: `NavItem[]` - Navigation items

**Features:**
- Sticky with blur on scroll
- Dropdown menus with glass effect
- Mobile responsive drawer
- Scroll-aware intensity

---

#### GlassTooltip

```tsx
import { GlassTooltip } from '@/components/GlassTooltip'

<GlassTooltip content="Helpful information" position="top" delay={200}>
  <button>Hover me</button>
</GlassTooltip>
```

**Props:**
- `content`: `React.ReactNode` - Tooltip content
- `position`: `'top' | 'bottom' | 'left' | 'right'` (default: `'top'`)
- `delay`: `number` - Show delay in ms (default: `200`)

---

### Vue 3 Components

#### GlassCard.vue

```vue
<template>
  <GlassCard variant="purple" intensity="lg" :hover="true" :glow="true">
    <h3 class="text-xl font-bold mb-2">Title</h3>
    <p class="text-white/80">Content goes here...</p>
  </GlassCard>
</template>

<script setup lang="ts">
import GlassCard from '@/components/GlassCard.vue'
</script>
```

---

#### GlassButton.vue

```vue
<template>
  <GlassButton
    variant="primary"
    size="md"
    :loading="isLoading"
    @click="handleClick"
  >
    Click Me
  </GlassButton>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import GlassButton from '@/components/GlassButton.vue'

const isLoading = ref(false)

const handleClick = async () => {
  isLoading.value = true
  await someAsyncOperation()
  isLoading.value = false
}
</script>
```

---

#### GlassInput.vue

```vue
<template>
  <GlassInput
    v-model="email"
    label="Email"
    type="email"
    :error="emailError"
    helper-text="Enter your email address"
  />
</template>

<script setup lang="ts">
import { ref } from 'vue'
import GlassInput from '@/components/GlassInput.vue'

const email = ref('')
const emailError = ref('')
</script>
```

---

## Tailwind Utilities

### Background Colors

```tsx
<div className="bg-glass-white">Light glass</div>
<div className="bg-glass-white-md">Medium light glass</div>
<div className="bg-glass-white-lg">Strong light glass</div>
<div className="bg-glass-purple">Purple glass</div>
<div className="bg-glass-blue">Blue glass</div>
<div className="bg-glass-cyan">Cyan glass</div>
```

### Backdrop Blur

```tsx
<div className="backdrop-blur-sm">Small blur</div>
<div className="backdrop-blur-md">Medium blur</div>
<div className="backdrop-blur-xl">Extra large blur</div>
<div className="backdrop-blur-2xl">2XL blur</div>
<div className="backdrop-blur-3xl">3XL blur</div>
```

### Border Colors

```tsx
<div className="border border-glass-white">Light border</div>
<div className="border border-glass-purple">Purple border</div>
```

### Glow Shadows

```tsx
<div className="shadow-glow">Basic glow</div>
<div className="shadow-glow-md">Medium glow</div>
<div className="shadow-glow-lg">Large glow</div>
<div className="shadow-glow-purple">Purple glow</div>
<div className="shadow-glow-blue">Blue glow</div>
```

### Custom Utilities

```tsx
<div className="text-glow">Glowing text</div>
<div className="gradient-border">Gradient border effect</div>
<div className="hover-glow">Glow on hover</div>
<div className="glass-scrollbar">Custom glass scrollbar</div>
```

---

## Design Tokens

### CSS Variables

```css
:root {
  /* Glass backgrounds */
  --glass-white: rgba(255, 255, 255, 0.1);
  --glass-white-md: rgba(255, 255, 255, 0.15);
  --glass-white-lg: rgba(255, 255, 255, 0.2);

  /* Colored glass */
  --glass-purple: rgba(147, 51, 234, 0.1);
  --glass-blue: rgba(59, 130, 246, 0.1);
  --glass-cyan: rgba(6, 182, 212, 0.1);

  /* Glow effects */
  --glow: 0 0 15px rgba(255, 255, 255, 0.15);
  --glow-md: 0 0 20px rgba(255, 255, 255, 0.2);
  --glow-lg: 0 0 30px rgba(255, 255, 255, 0.25);
}
```

---

## Best Practices

### 1. Always Use Dark Backgrounds

Glassmorphism requires a dark background to work properly:

```tsx
<body className="bg-gradient-to-br from-gray-900 via-purple-900 to-gray-900">
  {/* Glass components */}
</body>
```

### 2. Layer Glass Elements

Create depth by layering multiple glass elements:

```tsx
<GlassCard intensity="sm">
  <GlassCard intensity="md">
    <GlassCard intensity="lg">
      Deepest layer
    </GlassCard>
  </GlassCard>
</GlassCard>
```

### 3. Use Appropriate Blur Intensity

- **sm**: Subtle blur for text-heavy content
- **md**: Balanced blur for most use cases
- **lg**: Strong blur for backgrounds and overlays

### 4. Combine with Gradients

```tsx
<GlassCard className="bg-gradient-to-br from-purple-500/20 to-blue-500/20">
  Content with gradient background
</GlassCard>
```

### 5. Don't Overuse Glow Effects

Use glow sparingly for emphasis:

```tsx
// ❌ Too much glow
<GlassCard glow>
  <GlassButton className="shadow-glow">
    <span className="text-glow">Text</span>
  </GlassButton>
</GlassCard>

// ✅ Strategic glow
<GlassCard>
  <GlassButton>Click me</GlassButton>  {/* Glow on hover only */}
</GlassCard>
```

---

## Browser Support

### Backdrop Filter Support

Glassmorphism requires `backdrop-filter` support:

- ✅ Chrome 76+
- ✅ Safari 9+
- ✅ Firefox 103+
- ✅ Edge 79+

### Fallback for Unsupported Browsers

The system includes automatic fallbacks:

```css
@supports not (backdrop-filter: blur(10px)) {
  .glass-card {
    background-color: rgba(255, 255, 255, 0.15);
  }
}
```

---

## Accessibility

### Keyboard Navigation

All interactive components support keyboard navigation:
- **Tab**: Navigate between elements
- **Enter/Space**: Activate buttons
- **ESC**: Close modals

### ARIA Labels

Components include proper ARIA attributes:

```tsx
<GlassButton aria-label="Submit form">
  Submit
</GlassButton>
```

### Reduced Motion

Respects `prefers-reduced-motion`:

```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

### High Contrast Mode

Enhanced visibility in high contrast mode:

```css
@media (prefers-contrast: high) {
  .glass-card {
    border-width: 2px;
  }
}
```

---

## Examples

### Hero Section

```tsx
<section className="min-h-screen flex items-center justify-center p-8">
  <GlassCard variant="light" intensity="lg" glow className="max-w-2xl">
    <h1 className="text-5xl font-bold mb-4 text-glow">
      Welcome to the Future
    </h1>
    <p className="text-xl text-white/80 mb-8">
      Experience the next generation of web design
    </p>
    <GlassButton variant="primary" size="lg">
      Get Started
    </GlassButton>
  </GlassCard>
</section>
```

### Login Form

```tsx
<GlassCard variant="light" intensity="lg" className="max-w-md mx-auto p-8">
  <h2 className="text-3xl font-bold mb-6">Sign In</h2>
  <form className="space-y-4">
    <GlassInput
      label="Email"
      type="email"
      helperText="Enter your email address"
    />
    <GlassInput
      label="Password"
      type="password"
      helperText="At least 8 characters"
    />
    <GlassButton variant="primary" className="w-full">
      Sign In
    </GlassButton>
  </form>
</GlassCard>
```

### Dashboard

```tsx
<div className="min-h-screen">
  <GlassNavbar logo={<Logo />} items={navItems} />

  <div className="container mx-auto p-8 grid grid-cols-3 gap-6">
    <GlassCard variant="purple" hover glow>
      <h3 className="text-xl font-bold mb-2">Users</h3>
      <p className="text-4xl font-bold">1,234</p>
    </GlassCard>

    <GlassCard variant="blue" hover glow>
      <h3 className="text-xl font-bold mb-2">Revenue</h3>
      <p className="text-4xl font-bold">$56.7K</p>
    </GlassCard>

    <GlassCard variant="cyan" hover glow>
      <h3 className="text-xl font-bold mb-2">Growth</h3>
      <p className="text-4xl font-bold">+23%</p>
    </GlassCard>
  </div>
</div>
```

---

## Customization

### Extend Colors

Add custom glass colors in `tailwind.config.js`:

```js
backgroundColor: {
  'glass-orange': 'rgba(249, 115, 22, 0.1)',
  'glass-green': 'rgba(34, 197, 94, 0.1)',
},
borderColor: {
  'glass-orange': 'rgba(249, 115, 22, 0.3)',
  'glass-green': 'rgba(34, 197, 94, 0.3)',
},
boxShadow: {
  'glow-orange': '0 0 20px rgba(249, 115, 22, 0.4)',
  'glow-green': '0 0 20px rgba(34, 197, 94, 0.4)',
},
```

### Custom Blur Values

```js
backdropBlur: {
  '4xl': '80px',
  '5xl': '100px',
},
```

---

## Performance Tips

1. **Limit Blur Layers**: Too many blurred elements can impact performance
2. **Use will-change**: For animated glass elements
   ```css
   .glass-card {
     will-change: backdrop-filter;
   }
   ```
3. **Optimize Images**: Use compressed images behind glass
4. **Reduce Complexity**: Simplify DOM structure under glass elements

---

## Troubleshooting

### Blur Not Working

Check if `backdrop-filter` is supported:

```js
const supportsBackdrop = CSS.supports('backdrop-filter', 'blur(10px)')
console.log('Backdrop filter supported:', supportsBackdrop)
```

### Performance Issues

- Reduce number of blurred elements
- Lower blur intensity
- Use `backdrop-blur-md` instead of `backdrop-blur-3xl`

### Colors Look Washed Out

- Increase background darkness
- Use stronger glass variants (`-md`, `-lg`)
- Add gradient overlays

---

## Credits

**Design System:** Glassmorphism
**Autor:** {{AUTHOR}}
**Proyecto:** {{PROJECT_NAME}}
**Inspired by:** Apple iOS, Windows 11 Acrylic, Fluent Design

---

## License

Este diseño es parte de {{PROJECT_NAME}} y está disponible bajo la licencia del proyecto principal.
