# Design Systems

**Autor:** Homero Thompson del Lago del Terror

Esta carpeta contiene 5 sistemas de diseño modernos, cada uno optimizado para diferentes casos de uso y estéticas.

## Sistemas Disponibles

### 1. Vercel/Geist
Sistema minimalista inspirado en el diseño de Vercel. Enfoque en tipografía, espaciado y microinteracciones sutiles.

**Características:**
- Paleta monocromática con acentos sutiles
- Transiciones fluidas
- Optimizado para legibilidad
- Componentes limpios y funcionales

### 2. Glassmorphism
Efecto de vidrio esmerilado con fondos borrosos y transparencias.

**Características:**
- Blur y transparencia
- Bordes sutiles
- Profundidad visual
- Ideal para fondos coloridos

### 3. Neumorphism
Diseño soft-UI con sombras internas y externas para efecto 3D sutil.

**Características:**
- Elementos "salientes" del fondo
- Sombras suaves
- Paleta monocromática
- Aspecto táctil

### 4. Claymorphism
Estética de arcilla con sombras internas difusas y colores vibrantes.

**Características:**
- Fondos sólidos y vibrantes
- Sombras internas difusas
- Bordes redondeados
- Aspecto "inflado"

### 5. Neo-Brutalism
Diseño atrevido con bordes gruesos, sombras duras y colores contrastantes.

**Características:**
- Sombras duras (hard shadows)
- Bordes negros gruesos
- Colores saturados
- Tipografía bold

---

## Tabla Comparativa

| Sistema | Estilo | Mejor Para | Complejidad | Accesibilidad | Soporte |
|---------|--------|------------|-------------|---------------|---------|
| **Geist** | Minimalista | Apps corporativas, dashboards, SaaS | Baja | ★★★★★ | Todos los navegadores |
| **Glassmorphism** | Moderno, translúcido | Landing pages, portfolios, apps creativas | Media | ★★★★☆ | Chrome 76+, Safari 14+ |
| **Neumorphism** | Soft 3D | Apps móviles, widgets, controles táctiles | Media | ★★★☆☆ | Todos los navegadores |
| **Claymorphism** | Vibrante, táctil | Apps educativas, productos infantiles, juegos | Media | ★★★★☆ | Todos los navegadores |
| **Neo-Brutalism** | Atrevido, retro | Portfolios creativos, marcas jóvenes, arte | Baja | ★★★★★ | Todos los navegadores |

---

## Previsualización Visual

### Geist
- **Paleta:** Grises + azul neutro
- **Tipografía:** Inter Variable
- **Spacing:** 8px base
- **Bordes:** 8px radius
- **Sombras:** Suaves y elevadas

### Glassmorphism
- **Paleta:** Fondos gradientes + elementos translúcidos
- **Backdrop:** blur(10px)
- **Transparencia:** 0.1-0.3
- **Bordes:** 1px rgba con blur

### Neumorphism
- **Paleta:** Monocromática (base + 5% variación)
- **Sombras:** Dual (light + dark)
- **Bordes:** 12-20px radius
- **Efecto:** Inset para presionado

### Claymorphism
- **Paleta:** Colores vibrantes (HSL saturado)
- **Sombras:** Internas difusas
- **Bordes:** 16-24px radius
- **Textura:** Suave y mate

### Neo-Brutalism
- **Paleta:** Alto contraste (negro + colores primarios)
- **Bordes:** 3-4px negro sólido
- **Sombras:** Offset duro (4-8px)
- **Tipografía:** Bold, alta jerarquía

---

## Selección en el Wizard

Durante la ejecución de `/nuevo-proyecto`, el wizard preguntará:

```
¿Qué sistema de diseño prefieres para el frontend?
1. Vercel/Geist - Minimalismo profesional
2. Glassmorphism - Moderno y translúcido
3. Neumorphism - Soft 3D táctil
4. Claymorphism - Vibrante estilo arcilla
5. Neo-Brutalism - Atrevido y contrastante
```

El sistema seleccionado se aplicará a:
- Componentes UI (`packages/ui`)
- Variables CSS globales
- Tema de Tailwind
- Configuración de Storybook

---

## Instalación Manual

Si deseas cambiar el sistema de diseño después de la generación:

```bash
# 1. Copiar archivos del sistema deseado
cp -r frontend/design-systems/geist/components/* packages/ui/src/

# 2. Copiar configuración global
cp frontend/design-systems/geist/globals.css.tmpl apps/web/app/globals.css

# 3. Actualizar Tailwind config
cp frontend/design-systems/geist/tailwind.config.ts.tmpl tailwind.config.ts

# 4. Reinstalar dependencias si cambian
pnpm install
```

---

## Estructura por Sistema

Cada sistema de diseño incluye:

```
design-system-name/
├── README.md                    # Documentación específica
├── components/                  # Componentes React
│   ├── Button.tsx.tmpl
│   ├── Card.tsx.tmpl
│   ├── Input.tsx.tmpl
│   └── ...
├── globals.css.tmpl            # Variables CSS globales
├── tailwind.config.ts.tmpl     # Configuración de Tailwind
└── preview.png                 # Screenshot de referencia
```

---

## READMEs Individuales

Cada sistema tiene documentación detallada:

- [Geist](/frontend/design-systems/geist/README.md)
- [Glassmorphism](/frontend/design-systems/glassmorphism/README.md)
- [Neumorphism](/frontend/design-systems/neumorphism/README.md)
- [Claymorphism](/frontend/design-systems/claymorphism/README.md)
- [Neo-Brutalism](/frontend/design-systems/neo-brutalism/README.md)

---

## Guías Adicionales

- [Comparación Detallada](COMPARISON.md) - Casos de uso, pros/cons, performance
- [Design Tokens](design-tokens.ts.tmpl) - Tokens compartidos entre sistemas
- [Preview Data](preview-data.json.tmpl) - Datos para el wizard

---

## Contribuir

Al agregar un nuevo sistema de diseño:

1. Crear directorio con el nombre del sistema
2. Incluir README.md con ejemplos visuales
3. Implementar componentes base (Button, Card, Input, Badge, Modal)
4. Agregar configuración de Tailwind
5. Actualizar `index.ts.tmpl` con el nuevo sistema
6. Agregar entrada en `preview-data.json.tmpl`
7. Actualizar esta documentación

---

## Recursos Externos

- [Vercel Design System](https://vercel.com/design)
- [Glassmorphism.com](https://glassmorphism.com/)
- [Neumorphism.io](https://neumorphism.io/)
- [Hype4 Academy - Claymorphism](https://hype4.academy/tools/claymorphism-generator)
- [Brutalist Websites](https://brutalistwebsites.com/)

---

**Última actualización:** 2026-01-20
**Versión:** 6.1.0
