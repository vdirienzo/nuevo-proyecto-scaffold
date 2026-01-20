# Claymorphism Design System

![Claymorphism Banner](https://via.placeholder.com/1200x300/ff7aad/ffffff?text=Claymorphism+Design+System)

Un sistema de diseño moderno y amigable inspirado en el estilo Claymorphism (arcilla/plastilina 3D), con componentes para React y Vue 3.

**Autor:** Homero Thompson del Lago del Terror

---

## Características

- ✨ **Estilo Clay 3D** - Sombras dobles (externa + interna) para efecto de arcilla
- 🎨 **Paleta Pastel** - Colores cálidos y amigables
- 🔄 **Animaciones Playful** - Bounce, wobble, float y blob
- 🧩 **Componentes Completos** - 8 componentes React + 4 Vue 3
- 📱 **Responsive** - Diseñado mobile-first
- ♿ **Accesible** - ARIA labels, keyboard navigation
- 🎯 **TypeScript** - Totalmente tipado
- 🌈 **Variants** - 6 colores principales

---

## Instalación

### 1. Instalar dependencias

```bash
npm install tailwindcss @tailwindcss/forms
```

### 2. Configurar Tailwind

Copiar `tailwind.config.clay.js.tmpl` a tu proyecto como `tailwind.config.js`.

### 3. Importar estilos globales

```tsx
// En tu archivo principal (App.tsx o main.tsx)
import './globals.css'
```

---

## Componentes

### React

| Componente | Descripción | Props principales |
|------------|-------------|-------------------|
| `ClayButton` | Botón con efecto 3D | `variant`, `size`, `isLoading` |
| `ClayCard` | Tarjeta con blob decorativo | `variant`, `withBlob`, `hoverable` |
| `ClayInput` | Input con animación focus | `label`, `error`, `icon` |
| `ClayAvatar` | Avatar con status online | `size`, `online`, `fallback` |
| `ClayBadge` | Badge/Tag removable | `variant`, `removable`, `dot` |
| `ClayAlert` | Alerta/Toast con auto-close | `variant`, `autoClose`, `dismissible` |
| `ClayLoader` | Spinner animado | `variant`, `size`, `color` |
| `ClayAvatarGroup` | Grupo de avatares con overlap | `max`, `size` |

### Vue 3

| Componente | Descripción | Props principales |
|------------|-------------|-------------------|
| `ClayButton.vue` | Botón con efecto 3D | `variant`, `size`, `isLoading` |
| `ClayCard.vue` | Tarjeta con blob decorativo | `variant`, `withBlob`, `hoverable` |
| `ClayInput.vue` | Input con animación focus | `label`, `error`, `icon` |
| `ClayAvatar.vue` | Avatar con status online | `size`, `online`, `fallback` |

---

## Uso

### React

#### Button

```tsx
import { ClayButton } from './components/react/ClayButton'

function App() {
  return (
    <ClayButton variant="pink" size="md">
      Click me!
    </ClayButton>
  )
}
```

#### Card con Blob

```tsx
import { ClayCard } from './components/react/ClayCard'

function App() {
  return (
    <ClayCard
      variant="blue"
      withBlob
      blobPosition="top-right"
      hoverable
    >
      <h2>Card Title</h2>
      <p>Card content here</p>
    </ClayCard>
  )
}
```

#### Input

```tsx
import { ClayInput } from './components/react/ClayInput'

function App() {
  const [value, setValue] = useState('')

  return (
    <ClayInput
      label="Email"
      variant="blue"
      value={value}
      onChange={(e) => setValue(e.target.value)}
      placeholder="tu@email.com"
      icon={<MailIcon />}
    />
  )
}
```

#### Avatar Group

```tsx
import { ClayAvatar, ClayAvatarGroup } from './components/react/ClayAvatar'

function App() {
  return (
    <ClayAvatarGroup max={3}>
      <ClayAvatar src="/user1.jpg" online />
      <ClayAvatar src="/user2.jpg" />
      <ClayAvatar src="/user3.jpg" online />
      <ClayAvatar fallback="A" />
    </ClayAvatarGroup>
  )
}
```

#### Alert con Auto-close

```tsx
import { ClayAlert } from './components/react/ClayAlert'

function App() {
  return (
    <ClayAlert
      variant="success"
      title="¡Éxito!"
      autoClose
      autoCloseDelay={3000}
      dismissible
    >
      Tu operación se completó correctamente.
    </ClayAlert>
  )
}
```

#### Loader

```tsx
import { ClayLoader, ClayFullscreenLoader } from './components/react/ClayLoader'

function App() {
  return (
    <>
      {/* Inline loader */}
      <ClayLoader variant="dots" color="blue" size="md" />

      {/* Fullscreen loader */}
      <ClayFullscreenLoader
        variant="pulse"
        color="pink"
        message="Cargando tu contenido..."
      />
    </>
  )
}
```

### Vue 3

#### Button

```vue
<template>
  <ClayButton variant="pink" size="md">
    Click me!
  </ClayButton>
</template>

<script setup>
import ClayButton from './components/vue/ClayButton.vue'
</script>
```

#### Card con Blob

```vue
<template>
  <ClayCard
    variant="blue"
    :with-blob="true"
    blob-position="top-right"
    :hoverable="true"
  >
    <h2>Card Title</h2>
    <p>Card content here</p>
  </ClayCard>
</template>

<script setup>
import ClayCard from './components/vue/ClayCard.vue'
</script>
```

#### Input con v-model

```vue
<template>
  <ClayInput
    v-model="email"
    label="Email"
    variant="blue"
    placeholder="tu@email.com"
    :icon="mailIcon"
  />
</template>

<script setup>
import { ref } from 'vue'
import ClayInput from './components/vue/ClayInput.vue'

const email = ref('')
const mailIcon = '<svg>...</svg>' // Tu icono SVG
</script>
```

---

## Paleta de Colores

### Clay Colors

| Color | Hex | Uso |
|-------|-----|-----|
| Clay Pink | `#ff7aad` | Acciones principales, CTAs |
| Clay Blue | `#6096fa` | Información, links |
| Clay Green | `#4ade80` | Éxito, confirmación |
| Clay Yellow | `#facc15` | Advertencias, highlights |
| Clay Purple | `#c084fc` | Premium, especial |
| Clay Orange | `#fb923c` | Alerta urgente |

### Uso en CSS

```css
/* Usando variables CSS */
color: var(--clay-pink);
color: var(--clay-blue);

/* Usando Tailwind */
<div className="bg-clay-pink-400 text-white">
<div className="bg-clay-blue-100 text-gray-800">
```

---

## Sombras Clay

### Fórmula Base

```css
box-shadow:
  0 10px 30px rgba(0, 0, 0, 0.15),         /* Sombra exterior */
  inset 0 -5px 10px rgba(0, 0, 0, 0.05),  /* Sombra interior oscura */
  inset 0 5px 10px rgba(255, 255, 255, 0.5); /* Highlight interior */
```

### Variantes

| Clase | Uso |
|-------|-----|
| `shadow-clay` | Sombra base (elementos principales) |
| `shadow-clay-sm` | Sombra pequeña (badges, tags) |
| `shadow-clay-lg` | Sombra grande (modals, cards importantes) |
| `shadow-clay-inset` | Efecto presionado (inputs, botones activos) |
| `shadow-clay-hover` | Estado hover (interactivos) |

---

## Animaciones

### Built-in

| Animación | Uso | Clase |
|-----------|-----|-------|
| `bounce-soft` | Entrada de elementos | `animate-bounce-soft` |
| `wobble` | Interacción playful | `animate-wobble` |
| `float` | Elementos flotantes | `animate-float` |
| `blob` | Blobs decorativos | `animate-blob` |
| `pop` | Focus en inputs | `animate-pop` |

### Ejemplo personalizado

```tsx
<div className="animate-float hover:animate-wobble">
  Elemento playful
</div>
```

---

## Border Radius

| Clase | Valor | Uso |
|-------|-------|-----|
| `rounded-clay` | `24px` | Elementos principales |
| `rounded-blob` | `32px` | Cards, containers |
| `rounded-blob-lg` | `40px` | Headers, sections |

---

## Accesibilidad

### Focus States

Todos los componentes tienen estados de focus visibles:

```css
*:focus-visible {
  outline: none;
  ring: 2px solid var(--clay-blue-400);
  ring-offset: 2px;
}
```

### ARIA Labels

Todos los componentes interactivos incluyen ARIA labels apropiados:

```tsx
<button aria-label="Dismiss alert">
  <CloseIcon />
</button>
```

### Keyboard Navigation

- `Tab` - Navegar entre elementos
- `Enter/Space` - Activar botones
- `Escape` - Cerrar modales/alerts

---

## Temas y Personalización

### Cambiar colores principales

Editar `tailwind.config.clay.js`:

```js
theme: {
  extend: {
    colors: {
      clay: {
        pink: { /* tus colores */ },
        blue: { /* tus colores */ },
        // ...
      }
    }
  }
}
```

### Crear nuevas variantes

```tsx
// En ClayButton.tsx
const variantStyles = {
  // ... variantes existentes
  custom: 'bg-my-color hover:bg-my-color-dark text-white',
}
```

---

## Optimización

### Tree Shaking

Importa solo los componentes que uses:

```tsx
// ✅ Correcto
import { ClayButton } from './components/react/ClayButton'

// ❌ Evitar
import * as Clay from './components/react'
```

### Lazy Loading

```tsx
// React
const ClayCard = lazy(() => import('./components/react/ClayCard'))

// Vue
const ClayCard = defineAsyncComponent(() =>
  import('./components/vue/ClayCard.vue')
)
```

---

## Browser Support

| Browser | Version |
|---------|---------|
| Chrome | 90+ |
| Firefox | 88+ |
| Safari | 14+ |
| Edge | 90+ |

### Fallbacks

Para navegadores antiguos, las sombras clay degradan gracefully a sombras simples.

---

## Ejemplos Completos

### Landing Page

```tsx
import { ClayButton, ClayCard, ClayBadge } from './components/react'

function Hero() {
  return (
    <div className="clay-blob-bg min-h-screen flex items-center justify-center p-8">
      <ClayCard variant="white" withBlob hoverable padding="lg">
        <ClayBadge variant="pink" dot>Nuevo</ClayBadge>
        <h1 className="text-5xl font-bold mt-4 clay-text-gradient">
          Bienvenido a Claymorphism
        </h1>
        <p className="text-gray-600 mt-4 text-lg">
          Un sistema de diseño amigable y moderno
        </p>
        <div className="flex gap-4 mt-8">
          <ClayButton variant="pink" size="lg">
            Comenzar
          </ClayButton>
          <ClayButton variant="blue" size="lg">
            Ver demo
          </ClayButton>
        </div>
      </ClayCard>
    </div>
  )
}
```

### Dashboard

```tsx
import { ClayCard, ClayAvatar, ClayBadge, ClayLoader } from './components/react'

function Dashboard() {
  return (
    <div className="p-8 grid grid-cols-1 md:grid-cols-3 gap-6">
      <ClayCard variant="blue" hoverable>
        <div className="flex items-center justify-between">
          <div>
            <p className="text-gray-600">Total Users</p>
            <h3 className="text-3xl font-bold mt-2">1,234</h3>
          </div>
          <ClayBadge variant="green" dot>+12%</ClayBadge>
        </div>
      </ClayCard>

      <ClayCard variant="pink" hoverable>
        <div className="flex items-center justify-between">
          <div>
            <p className="text-gray-600">Revenue</p>
            <h3 className="text-3xl font-bold mt-2">$45.2K</h3>
          </div>
          <ClayBadge variant="yellow" dot>+5%</ClayBadge>
        </div>
      </ClayCard>

      <ClayCard variant="green" hoverable>
        <div className="flex items-center justify-between">
          <div>
            <p className="text-gray-600">Active Now</p>
            <h3 className="text-3xl font-bold mt-2">89</h3>
          </div>
          <ClayLoader variant="pulse" color="green" size="sm" />
        </div>
      </ClayCard>
    </div>
  )
}
```

---

## FAQ

### ¿Puedo usar con Next.js?

Sí, importa los estilos globales en `_app.tsx`:

```tsx
import '../styles/globals.css'
```

### ¿Funciona con dark mode?

El sistema está diseñado para light mode, pero puedes agregar variantes dark editando `globals.css`.

### ¿Puedo usar con otros frameworks?

Sí, los estilos Tailwind son framework-agnostic. Solo adapta los componentes al framework de tu elección.

### ¿Cómo contribuir?

Este sistema es parte del scaffold {{PROJECT_NAME}}. Para contribuir, envía PRs al repositorio principal.

---

## Changelog

### v1.0.0 (2026-01-20)

- ✨ Lanzamiento inicial
- 🎨 8 componentes React
- 🎨 4 componentes Vue 3
- 📚 Documentación completa
- ♿ Soporte de accesibilidad
- 📱 Responsive design

---

## Licencia

Ver LICENSE en el repositorio principal.

---

## Autor

**Homero Thompson del Lago del Terror**

Creado con ❤️ para {{PROJECT_NAME}}

---

## Links

- [Tailwind CSS](https://tailwindcss.com)
- [React](https://react.dev)
- [Vue 3](https://vuejs.org)
- [Claymorphism Design Trend](https://uxdesign.cc/claymorphism-in-user-interfaces-1f64d6e3f0e0)
