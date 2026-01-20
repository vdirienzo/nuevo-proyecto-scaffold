# Design System Shared Utilities

Shared utilities, hooks, and components for all design systems in the scaffold.

**Autor:** Homero Thompson del Lago del Terror

## Overview

This directory contains framework-agnostic utilities and framework-specific hooks that work across all design systems (Glassmorphism, Neumorphism, Neo-Brutalism, Claymorphism, and Geist).

## Directory Structure

```
shared/
├── hooks/              # React hooks
│   ├── useDesignSystem.ts.tmpl
│   └── useTheme.ts.tmpl
├── composables/        # Vue 3 composables
│   ├── useDesignSystem.ts.tmpl
│   └── useTheme.ts.tmpl
├── context/            # React context providers
│   └── DesignSystemProvider.tsx.tmpl
├── utils/              # Framework-agnostic utilities
│   ├── cn.ts.tmpl      # Class name merging
│   └── animations.ts.tmpl  # Framer Motion variants
├── types/              # TypeScript types
│   └── index.ts.tmpl
└── constants.ts.tmpl   # Shared constants
```

## Usage Examples

### React

```tsx
// App.tsx - Setup provider
import { DesignSystemProvider } from './shared/context/DesignSystemProvider';
import glassmorphismConfig from './glassmorphism/config';

function App() {
  return (
    <DesignSystemProvider config={glassmorphismConfig}>
      <YourApp />
    </DesignSystemProvider>
  );
}

// Component.tsx - Use hooks
import { useDesignSystem, useTheme } from './shared/hooks';

function Button() {
  const { tokens, components } = useDesignSystem();
  const { isDark, toggleTheme } = useTheme();

  return (
    <button
      className={components.button.variants.primary}
      onClick={toggleTheme}
    >
      {isDark ? 'Light Mode' : 'Dark Mode'}
    </button>
  );
}
```

### Vue 3

```vue
<!-- App.vue - Provide design system -->
<script setup lang="ts">
import { provide, ref } from 'vue';
import { DESIGN_SYSTEM_KEY } from './shared/composables/useDesignSystem';
import glassmorphismConfig from './glassmorphism/config';

provide(DESIGN_SYSTEM_KEY, {
  config: ref(glassmorphismConfig),
  tokens: ref(glassmorphismConfig.tokens),
  components: ref(glassmorphismConfig.components),
});
</script>

<!-- Component.vue - Use composables -->
<script setup lang="ts">
import { useDesignSystem, useTheme } from './shared/composables';

const { tokens, components } = useDesignSystem();
const { isDark, toggleTheme } = useTheme();
</script>

<template>
  <button :class="components.button.variants.primary" @click="toggleTheme">
    {{ isDark ? 'Light Mode' : 'Dark Mode' }}
  </button>
</template>
```

## Features

### 🎨 Design System Access
- Get tokens (colors, spacing, typography)
- Access component styles
- Check current design system
- Switch design systems at runtime (if enabled)

### 🌓 Theme Management
- Light/dark mode toggle
- System preference detection
- LocalStorage persistence
- SSR-safe implementation

### 🎭 Class Name Utilities
- Merge Tailwind classes intelligently
- Variant-based class builders
- Responsive class helpers
- Conditional class application

### 🎬 Animations
- Framer Motion variants for each design system
- Common entrance/exit animations
- Stagger children utilities
- Modal/dialog animations

## Dependencies

### Required
```json
{
  "clsx": "^2.0.0",
  "tailwind-merge": "^2.0.0"
}
```

### Optional (for animations)
```json
{
  "framer-motion": "^10.0.0"
}
```

## TypeScript Support

All utilities are fully typed with TypeScript. Import types:

```typescript
import type {
  DesignSystemConfig,
  DesignTokens,
  ComponentStyles,
  DesignSystemName,
} from './shared/types';
```

## SSR Considerations

To prevent flash of unstyled content (FOUC) in SSR applications:

```tsx
// React (Next.js)
import { themeScript } from './shared/hooks/useTheme';

export default function Document() {
  return (
    <Html>
      <Head>
        <script dangerouslySetInnerHTML={{ __html: themeScript }} />
      </Head>
      <body>
        <Main />
      </body>
    </Html>
  );
}
```

```vue
<!-- Vue (Nuxt) -->
<script setup>
import { themeScript } from './shared/composables/useTheme';
useHead({
  script: [{ innerHTML: themeScript, type: 'text/javascript' }],
});
</script>
```

## Variables

These templates support the following variables:

- `{{PROJECT_NAME}}` - Used in storage keys
- `{{AUTHOR}}` - Used in file headers

## Contributing

When adding new utilities:

1. Keep them framework-agnostic when possible
2. Provide both React and Vue versions for hooks
3. Update TypeScript types
4. Add usage examples in this README
5. Ensure SSR compatibility

## License

Part of the Nuevo Proyecto Scaffold by Homero Thompson del Lago del Terror.
