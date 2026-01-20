# Neo-Brutalism Design System

> **Autor:** Homero Thompson del Lago del Terror
> **Project:** {{PROJECT_NAME}}

Un sistema de diseño Neo-Brutal completo con componentes para React y Vue 3, que abraza la estética "feo a propósito" con colores brillantes, tipografía chunky y sombras duras.

---

## Características

- 🎨 **Paleta de colores brillantes** - Yellow, magenta, cyan, lime, orange
- 🔲 **Bordes gruesos** - 4px, 6px, 8px
- 🪨 **Sombras duras** - Sin blur, solo offset (4px 4px, 8px 8px)
- 📐 **Layouts asimétricos** - Rotaciones, overlapping, chaos
- 🔤 **Tipografía chunky** - Space Grotesk + Clash Display
- ⚛️ **Componentes React y Vue 3** - TypeScript completo
- 🎭 **Animaciones brutales** - Shake, marquee, hover effects

---

## Instalación

### 1. Instalar dependencias

```bash
npm install tailwindcss class-variance-authority clsx tailwind-merge
```

### 2. Copiar configuración

Copiar `tailwind.config.brutal.js.tmpl` a `tailwind.config.js` en tu proyecto:

```bash
cp tailwind.config.brutal.js.tmpl tailwind.config.js
```

### 3. Importar estilos globales

En tu archivo CSS principal (ej: `globals.css`):

```css
@import './globals.css';
```

O copiar el contenido de `globals.css.tmpl` directamente.

### 4. Copiar componentes

Copiar los componentes React o Vue a tu proyecto:

```bash
# React
cp -r components/react/* src/components/

# Vue
cp -r components/vue/* src/components/
```

---

## Paleta de Colores

| Color | Hex | Clase Tailwind | Uso |
|-------|-----|----------------|-----|
| Yellow | `#FFFF00` | `bg-brutal-yellow` | Primario, llamadas a la acción |
| Magenta | `#FF00FF` | `bg-brutal-magenta` | Secundario, alertas |
| Cyan | `#00FFFF` | `bg-brutal-cyan` | Información, links |
| Lime | `#00FF00` | `bg-brutal-lime` | Éxito, confirmaciones |
| Orange | `#FF6600` | `bg-brutal-orange` | Advertencias |
| Red | `#FF0000` | `bg-brutal-red` | Errores, destructivos |
| Black | `#000000` | `bg-brutal-black` | Texto, bordes |
| White | `#FFFFFF` | `bg-brutal-white` | Fondos, texto en oscuro |

---

## Tipografía

### Fuentes

- **Space Grotesk** - Monoespaciada chunky para UI
- **Clash Display** - Sans-serif bold para headings

### Tamaños

| Tamaño | Clase | Uso |
|--------|-------|-----|
| 3xl | `text-brutal-3xl` | Headings pequeños |
| 4xl | `text-brutal-4xl` | Headings medianos |
| 5xl | `text-brutal-5xl` | Headings grandes |
| 6xl | `text-brutal-6xl` | Headings extra grandes |
| 7xl | `text-brutal-7xl` | Hero headings |
| 8xl | `text-brutal-8xl` | Display |
| 9xl | `text-brutal-9xl` | Display masivo |

---

## Sombras

### Sombras Hard (sin blur)

```css
box-shadow: 4px 4px 0px #000000; /* shadow-brutal-sm */
box-shadow: 6px 6px 0px #000000; /* shadow-brutal-md */
box-shadow: 8px 8px 0px #000000; /* shadow-brutal-lg */
box-shadow: 12px 12px 0px #000000; /* shadow-brutal-xl */
```

### Sombras de color

```css
shadow-brutal-yellow-sm   /* 4px 4px 0px #FFFF00 */
shadow-brutal-magenta-md  /* 8px 8px 0px #FF00FF */
shadow-brutal-cyan-sm     /* 4px 4px 0px #00FFFF */
shadow-brutal-lime-md     /* 8px 8px 0px #00FF00 */
```

---

## Componentes React

### BrutalButton

Botón con sombra dura y efecto pressed.

```tsx
import BrutalButton from '@/components/BrutalButton'

<BrutalButton
  variant="yellow"
  size="md"
  uppercase
  shadowColor="black"
>
  Click Me
</BrutalButton>
```

**Props:**
- `variant`: 'yellow' | 'magenta' | 'cyan' | 'lime' | 'orange' | 'red' | 'blue' | 'pink'
- `size`: 'sm' | 'md' | 'lg' | 'xl'
- `uppercase`: boolean
- `shadowColor`: 'black' | 'yellow' | 'magenta' | 'cyan' | 'lime'

---

### BrutalCard

Card con borde grueso, sombra y stickers opcionales.

```tsx
import BrutalCard from '@/components/BrutalCard'

<BrutalCard
  variant="white"
  shadow="md"
  asymmetric={false}
  rotate={3}
  sticker={{
    text: 'NEW',
    color: 'red',
    position: 'top-right'
  }}
>
  <h3>Card Title</h3>
  <p>Card content...</p>
</BrutalCard>
```

**Props:**
- `variant`: 'white' | 'yellow' | 'magenta' | 'cyan' | 'lime' | 'orange'
- `shadow`: 'sm' | 'md' | 'lg' | 'xl'
- `asymmetric`: boolean - Aplica skew
- `rotate`: number - Grados de rotación
- `sticker`: { text, color, position }

---

### BrutalInput

Input con focus de alto contraste y error shake.

```tsx
import BrutalInput from '@/components/BrutalInput'

<BrutalInput
  label="Email"
  placeholder="your@email.com"
  focusColor="cyan"
  error={errors.email}
/>
```

**Props:**
- `label`: string
- `error`: string - Muestra error box y shake
- `focusColor`: 'yellow' | 'magenta' | 'cyan' | 'lime'

---

### BrutalHeading

Heading masivo con highlight y strikethrough.

```tsx
import BrutalHeading from '@/components/BrutalHeading'

<BrutalHeading
  as="h1"
  size="4xl"
  uppercase
  rotate={-3}
  highlight={{
    text: 'BRUTAL',
    color: 'yellow'
  }}
>
  THIS IS BRUTAL DESIGN
</BrutalHeading>
```

**Props:**
- `as`: 'h1' | 'h2' | 'h3' | 'h4' | 'h5' | 'h6'
- `size`: 'sm' | 'md' | 'lg' | 'xl' | '2xl' | '3xl' | '4xl' | '5xl' | '6xl'
- `highlight`: { text, color } - Efecto marker
- `strikethrough`: { text } - Tachado brutal
- `rotate`: number
- `uppercase`: boolean

---

### BrutalLink

Link con underline grueso y arrow animado.

```tsx
import BrutalLink from '@/components/BrutalLink'

<BrutalLink
  href="https://example.com"
  underlineColor="magenta"
  showArrow
  external
>
  Visit Site
</BrutalLink>
```

**Props:**
- `underlineColor`: 'yellow' | 'magenta' | 'cyan' | 'lime' | 'black'
- `showArrow`: boolean
- `external`: boolean - Abre en nueva pestaña

---

### BrutalMarquee

Texto scrolling infinito.

```tsx
import BrutalMarquee from '@/components/BrutalMarquee'

<BrutalMarquee
  text="HOT SALE"
  separator="★"
  speed="fast"
  bgColor="magenta"
  textColor="black"
  uppercase
/>
```

**Props:**
- `text`: string
- `separator`: string (default '★')
- `speed`: 'slow' | 'normal' | 'fast'
- `bgColor`: 'yellow' | 'magenta' | 'cyan' | 'lime' | 'orange' | 'black'
- `textColor`: 'black' | 'white'

---

### BrutalSticker

Decoración tipo badge/starburst.

```tsx
import BrutalSticker from '@/components/BrutalSticker'

<BrutalSticker
  text="NEW"
  variant="starburst"
  color="red"
  size="md"
  rotate={12}
/>
```

**Props:**
- `text`: string
- `variant`: 'badge' | 'starburst' | 'circle'
- `color`: 'yellow' | 'magenta' | 'cyan' | 'lime' | 'orange' | 'red'
- `size`: 'sm' | 'md' | 'lg'
- `rotate`: number

---

### BrutalGrid

Grid asimétrico con layouts caóticos.

```tsx
import BrutalGrid from '@/components/BrutalGrid'

<BrutalGrid
  layout="chaos"
  columns={3}
  gap="lg"
>
  <BrutalCard>Item 1</BrutalCard>
  <BrutalCard>Item 2</BrutalCard>
  <BrutalCard>Item 3</BrutalCard>
</BrutalGrid>
```

**Props:**
- `layout`: 'asymmetric' | 'masonry' | 'overlap' | 'chaos'
- `columns`: 2 | 3 | 4
- `gap`: 'sm' | 'md' | 'lg' | 'xl'

---

## Componentes Vue 3

Los componentes Vue tienen las mismas props y funcionalidad que los de React, pero usando Composition API.

### Ejemplo BrutalButton.vue

```vue
<template>
  <BrutalButton
    variant="cyan"
    size="lg"
    :uppercase="true"
    @click="handleClick"
  >
    Click Me
  </BrutalButton>
</template>

<script setup lang="ts">
import BrutalButton from '@/components/BrutalButton.vue'

const handleClick = () => {
  console.log('Clicked!')
}
</script>
```

### Ejemplo BrutalInput.vue con v-model

```vue
<template>
  <BrutalInput
    v-model="email"
    label="Email"
    :error="emailError"
    focus-color="magenta"
  />
</template>

<script setup lang="ts">
import { ref } from 'vue'
import BrutalInput from '@/components/BrutalInput.vue'

const email = ref('')
const emailError = ref('')
</script>
```

---

## Utilidades CSS

### Marker Highlight

```tsx
<span className="marker-highlight">Highlighted text</span>
<span className="marker-highlight-magenta">Magenta highlight</span>
<span className="marker-highlight-cyan">Cyan highlight</span>
```

### Underline Brutal

```tsx
<span className="text-underline-brutal">Thick underline</span>
<span className="wavy-underline">Wavy underline</span>
```

### Strikethrough Brutal

```tsx
<span className="text-strikethrough-brutal">Crossed out</span>
```

### Animaciones

```tsx
<div className="animate-shake">Shake on error</div>
<div className="animate-marquee">Scroll horizontal</div>
```

---

## Principios de Diseño

### 1. Alto Contraste
Usa colores brillantes que griten. Negro (#000000) para bordes y texto, colores neón para fondos.

### 2. Bordes Gruesos
Mínimo 4px, ideal 6px para elementos importantes.

### 3. Sombras Sin Blur
`box-shadow: 4px 4px 0px #000000` - Hard drop shadows solamente.

### 4. Tipografía Chunky
Usa pesos bold (700+), ALL CAPS para énfasis, tamaños grandes.

### 5. Asimetría Controlada
Rotaciones pequeñas (2-6deg), offsets, overlapping, pero mantén legibilidad.

### 6. Interacciones Obvias
Botones con efecto pressed (shadow desaparece), hovers evidentes, shake en errores.

---

## Ejemplos de Uso

### Landing Page Hero

```tsx
import { BrutalHeading, BrutalButton, BrutalMarquee } from '@/components'

export default function Hero() {
  return (
    <section className="min-h-screen bg-brutal-yellow flex items-center justify-center">
      <div className="text-center">
        <BrutalHeading
          as="h1"
          size="5xl"
          uppercase
          highlight={{ text: 'BRUTAL', color: 'magenta' }}
        >
          THIS IS BRUTAL DESIGN
        </BrutalHeading>
        <p className="text-brutal-2xl font-bold mt-6">
          Ugly on purpose. High contrast. Chunky AF.
        </p>
        <BrutalButton
          variant="magenta"
          size="xl"
          uppercase
          shadowColor="black"
          className="mt-8"
        >
          Get Started
        </BrutalButton>
      </div>
      <BrutalMarquee
        text="NEW BRUTAL DESIGN SYSTEM"
        bgColor="black"
        textColor="white"
        className="absolute bottom-0"
      />
    </section>
  )
}
```

### Form con Validación

```tsx
import { useState } from 'react'
import { BrutalInput, BrutalButton, BrutalCard } from '@/components'

export default function ContactForm() {
  const [email, setEmail] = useState('')
  const [error, setError] = useState('')

  const handleSubmit = (e) => {
    e.preventDefault()
    if (!email.includes('@')) {
      setError('Invalid email format')
      return
    }
    // Submit...
  }

  return (
    <BrutalCard
      variant="cyan"
      shadow="lg"
      sticker={{ text: 'CONTACT', color: 'magenta', position: 'top-right' }}
    >
      <h2 className="text-brutal-4xl font-clash font-bold mb-6">
        GET IN TOUCH
      </h2>
      <form onSubmit={handleSubmit} className="space-y-4">
        <BrutalInput
          label="Email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          error={error}
          focusColor="magenta"
        />
        <BrutalButton
          type="submit"
          variant="magenta"
          size="lg"
          uppercase
          className="w-full"
        >
          Submit
        </BrutalButton>
      </form>
    </BrutalCard>
  )
}
```

### Grid Asimétrico

```tsx
import { BrutalGrid, BrutalCard, BrutalSticker } from '@/components'

export default function ProductGrid() {
  const products = [
    { id: 1, name: 'Product A', new: true },
    { id: 2, name: 'Product B', new: false },
    { id: 3, name: 'Product C', new: true },
  ]

  return (
    <BrutalGrid layout="chaos" columns={3} gap="lg">
      {products.map(product => (
        <BrutalCard
          key={product.id}
          variant="white"
          shadow="md"
          sticker={product.new ? {
            text: 'NEW',
            color: 'red',
            position: 'top-right'
          } : undefined}
        >
          <h3 className="text-brutal-2xl font-bold">{product.name}</h3>
          <p className="mt-2">Description...</p>
        </BrutalCard>
      ))}
    </BrutalGrid>
  )
}
```

---

## Personalización

### Agregar Nuevos Colores

En `tailwind.config.brutal.js`:

```js
colors: {
  brutal: {
    // ... colores existentes
    purple: '#9900FF',
    green: '#00CC00',
  }
}
```

### Agregar Nuevas Sombras

```js
boxShadow: {
  // ... sombras existentes
  'brutal-purple-md': '8px 8px 0px #9900FF',
}
```

### Agregar Nuevas Fuentes

En `globals.css`:

```css
@import url('https://fonts.googleapis.com/css2?family=Your+Font:wght@700&display=swap');
```

En `tailwind.config.brutal.js`:

```js
fontFamily: {
  custom: ['Your Font', 'sans-serif'],
}
```

---

## Recursos

- [Space Grotesk Font](https://fonts.google.com/specimen/Space+Grotesk)
- [Clash Display Font](https://www.fontshare.com/fonts/clash-display)
- [Neo-Brutalism Web Design](https://hype4.academy/articles/design/brutalism-in-web-design)

---

## Changelog

### 1.0.0 - 2025-01-20

- Initial release
- 8 componentes React
- 4 componentes Vue 3
- Tailwind config completo
- Globals CSS con animaciones
- Documentación completa

---

## Autor

**Homero Thompson del Lago del Terror**

---

## Licencia

Este sistema de diseño es parte de {{PROJECT_NAME}} y sigue la licencia del proyecto.
