# Design Systems - Comparación Detallada

**Autor:** Homero Thompson del Lago del Terror

Esta guía te ayuda a elegir el sistema de diseño adecuado según el contexto, requisitos y objetivos del proyecto.

---

## Cuándo Usar Cada Sistema

### Geist (Vercel/Minimalist)

**Usa cuando:**
- El proyecto requiere máxima legibilidad y claridad
- La audiencia es técnica o corporativa
- Necesitas alta accesibilidad (WCAG 2.1 AAA)
- El foco está en contenido y funcionalidad
- Quieres reducir distracciones visuales
- El proyecto es a largo plazo (diseño atemporal)

**Evita cuando:**
- Necesitas diferenciación visual fuerte
- El proyecto es lúdico o creativo
- La marca requiere personalidad audaz

**Ejemplos de uso ideal:**
- SaaS B2B dashboards
- Plataformas de documentación
- Herramientas de productividad
- Aplicaciones financieras
- Paneles de administración

---

### Glassmorphism

**Usa cuando:**
- El diseño tiene fondos visuales ricos (fotos, gradientes)
- Quieres crear jerarquía visual con profundidad
- El público es joven y tech-savvy
- La marca es moderna y premium
- El contenido es visual (portfolios, galerías)

**Evita cuando:**
- La accesibilidad es crítica (bajo contraste puede ser problema)
- Los usuarios tienen hardware antiguo (blur es costoso)
- Necesitas soporte para IE11 o navegadores viejos
- El contenido es denso en texto

**Ejemplos de uso ideal:**
- Landing pages de productos tech
- Portfolios creativos
- Apps de música/streaming
- Interfaces de gaming
- Dashboards de analytics con visualizaciones

**Consideraciones técnicas:**
- `backdrop-filter: blur()` puede causar lag en móviles de gama baja
- Requiere testing exhaustivo de contraste (WCAG)
- Animaciones de blur son costosas en performance

---

### Neumorphism

**Usa cuando:**
- El proyecto es para móvil (táctil)
- Diseñas controles físicos (players de música, termostatos)
- Quieres un look minimalista pero diferente
- La interfaz tiene pocos elementos grandes
- El público aprecia diseño sutil

**Evita cuando:**
- La accesibilidad es prioritaria (bajo contraste inherente)
- Hay muchos elementos pequeños (se pierde el efecto)
- El diseño es denso en información
- Necesitas alto contraste para texto

**Ejemplos de uso ideal:**
- Widgets de sistema
- Apps de smart home
- Reproductores de audio
- Controles de IoT
- Interfaces de wearables

**Consideraciones técnicas:**
- Requiere fondo monocromático
- Mal contraste puede fallar WCAG AA
- Las sombras múltiples aumentan el DOM/CSS
- Difícil de hacer responsivo

---

### Claymorphism

**Usa cuando:**
- El público objetivo es joven o infantil
- El proyecto es educativo o lúdico
- La marca es alegre y vibrante
- Quieres crear emociones positivas
- El diseño incluye gamificación

**Evita cuando:**
- El proyecto es corporativo o formal
- La audiencia es conservadora
- Necesitas look profesional
- El contenido es serio (finanzas, legal, salud)

**Ejemplos de uso ideal:**
- Apps educativas para niños
- Juegos casuales
- Plataformas de creatividad
- Apps de fitness/wellness
- Herramientas de team building

**Consideraciones técnicas:**
- Colores vibrantes deben testearse para accesibilidad
- Puede parecer infantil si no se balancea bien
- Requiere buen criterio en uso de color

---

### Neo-Brutalism

**Usa cuando:**
- La marca es atrevida y contracultural
- El público es creativo (artistas, diseñadores)
- Quieres destacar entre competencia genérica
- El proyecto es portfolio o comunidad
- La diferenciación visual es clave

**Evita cuando:**
- El proyecto es corporativo conservador
- La audiencia es mayor o no tech-savvy
- Necesitas diseño sutil y discreto
- El cliente no tolera riesgos

**Ejemplos de uso ideal:**
- Portfolios de diseñadores
- Agencias creativas
- Comunidades artísticas
- NFT/Web3 projects
- Marcas de streetwear/moda

**Consideraciones técnicas:**
- Alto contraste = excelente para accesibilidad
- Sombras hard son baratas (no blur)
- Fácil de implementar (CSS simple)
- Requiere buen criterio tipográfico

---

## Matriz de Decisión

| Criterio | Geist | Glassmorphism | Neumorphism | Claymorphism | Neo-Brutalism |
|----------|-------|---------------|-------------|--------------|---------------|
| **Audiencia corporativa** | ★★★★★ | ★★☆☆☆ | ★★★☆☆ | ★☆☆☆☆ | ★☆☆☆☆ |
| **Audiencia joven** | ★★★☆☆ | ★★★★★ | ★★★☆☆ | ★★★★★ | ★★★★★ |
| **Audiencia infantil** | ★★☆☆☆ | ★★☆☆☆ | ★★★☆☆ | ★★★★★ | ★☆☆☆☆ |
| **Accesibilidad WCAG AAA** | ★★★★★ | ★★★☆☆ | ★★☆☆☆ | ★★★★☆ | ★★★★★ |
| **Performance móvil** | ★★★★★ | ★★☆☆☆ | ★★★★☆ | ★★★★☆ | ★★★★★ |
| **Facilidad implementación** | ★★★★★ | ★★★☆☆ | ★★★☆☆ | ★★★☆☆ | ★★★★★ |
| **Diferenciación visual** | ★★☆☆☆ | ★★★★☆ | ★★★★☆ | ★★★★☆ | ★★★★★ |
| **Longevidad/atemporal** | ★★★★★ | ★★★☆☆ | ★★☆☆☆ | ★★☆☆☆ | ★★★☆☆ |
| **Contenido denso** | ★★★★★ | ★★☆☆☆ | ★★☆☆☆ | ★★☆☆☆ | ★★★★☆ |
| **Visual storytelling** | ★★★☆☆ | ★★★★★ | ★★☆☆☆ | ★★★★☆ | ★★★★☆ |

---

## Pros y Contras

### Geist

**Pros:**
- Accesibilidad perfecta out-of-the-box
- Performance óptima (CSS simple)
- Fácil de mantener
- Diseño atemporal
- Excelente para contenido
- Alto contraste
- Tipografía optimizada

**Contras:**
- Puede parecer genérico
- Poca diferenciación visual
- Menos "wow factor"
- No ideal para brands con personalidad fuerte

---

### Glassmorphism

**Pros:**
- Estéticamente moderno
- Profundidad visual sin complejidad
- Flexible con fondos
- Premium look
- Trending design

**Contras:**
- Problemas de accesibilidad (contraste)
- Performance impacto (blur)
- Soporte navegadores limitado
- Difícil de hacer bien
- Puede verse dated en 2-3 años

---

### Neumorphism

**Pros:**
- Único y memorable
- Táctil y satisfactorio
- Minimalista pero distinto
- Bueno para controles físicos

**Contras:**
- Accesibilidad pobre (contraste)
- Difícil de escalar
- Requiere fondo específico
- No apto para interfaces densas
- Trendy (puede envejecer mal)

---

### Claymorphism

**Pros:**
- Alegre y positivo
- Memorable
- Bueno para engagement
- Flexible con color
- Accesibilidad decente

**Contras:**
- No apropiado para contextos serios
- Puede verse infantil
- Requiere buen gusto en color
- Menos profesional

---

### Neo-Brutalism

**Pros:**
- Altamente distintivo
- Accesibilidad excelente
- Performance óptima
- Fácil de implementar
- Atemporal (retro-futurista)
- Bold y memorable

**Contras:**
- No apto para corporativo
- Polarizante (love it or hate it)
- Requiere confianza del cliente
- Puede cansar visualmente

---

## Soporte de Navegadores

### Geist
- Chrome/Edge: ✅ Todas las versiones
- Safari: ✅ Todas las versiones
- Firefox: ✅ Todas las versiones
- IE11: ✅ Con polyfills menores

### Glassmorphism
- Chrome/Edge: ✅ 76+ (backdrop-filter)
- Safari: ✅ 14+ (prefijo -webkit)
- Firefox: ✅ 103+
- IE11: ❌ No soportado

**Fallback strategy:** Fondo sólido con opacidad reducida.

### Neumorphism
- Chrome/Edge: ✅ Todas las versiones
- Safari: ✅ Todas las versiones
- Firefox: ✅ Todas las versiones
- IE11: ✅ Con polyfills para CSS custom properties

### Claymorphism
- Chrome/Edge: ✅ Todas las versiones
- Safari: ✅ Todas las versiones
- Firefox: ✅ Todas las versiones
- IE11: ⚠️ Requiere polyfills (backdrop-filter para blur)

### Neo-Brutalism
- Chrome/Edge: ✅ Todas las versiones
- Safari: ✅ Todas las versiones
- Firefox: ✅ Todas las versiones
- IE11: ✅ 100% compatible

---

## Consideraciones de Performance

### Bundle Size (CSS)

| Sistema | Tailwind Classes | Custom CSS | Total Estimado |
|---------|------------------|------------|----------------|
| Geist | ~12KB | ~2KB | ~14KB |
| Glassmorphism | ~15KB | ~5KB | ~20KB |
| Neumorphism | ~14KB | ~8KB | ~22KB |
| Claymorphism | ~16KB | ~6KB | ~22KB |
| Neo-Brutalism | ~10KB | ~3KB | ~13KB |

### Runtime Performance

**Geist:** ★★★★★ (CSS simple, sin efectos costosos)

**Glassmorphism:** ★★☆☆☆
- `backdrop-filter: blur()` es costoso
- Re-render de blur en animaciones = lag
- Especialmente lento en móviles low-end

**Neumorphism:** ★★★★☆
- Múltiples box-shadows pueden acumular
- Pero sin blur = mejor que glassmorphism

**Claymorphism:** ★★★★☆
- Similar a neumorphism
- Blur en fondos puede ser costoso

**Neo-Brutalism:** ★★★★★
- Hard shadows = muy baratas
- Sin blur, sin gradientes complejos
- Performance óptima

### Recomendación por dispositivo

- **Desktop high-end:** Cualquiera
- **Desktop mid-range:** Evitar glassmorphism pesado
- **Móvil high-end:** Cualquiera con testing
- **Móvil mid/low-range:** Geist o Neo-Brutalism
- **Smartwatches/IoT:** Neumorphism o Geist

---

## Scores de Accesibilidad

### Contraste de Texto (WCAG 2.1)

| Sistema | AA (4.5:1) | AAA (7:1) | Notas |
|---------|------------|-----------|-------|
| Geist | ✅ Pass | ✅ Pass | Negro sobre blanco = ratio 21:1 |
| Glassmorphism | ⚠️ Cuidado | ❌ Difícil | Requiere testing caso por caso |
| Neumorphism | ⚠️ Cuidado | ❌ Raro | Contraste inherentemente bajo |
| Claymorphism | ✅ Pass | ⚠️ Depende | Con colores correctos pasa |
| Neo-Brutalism | ✅ Pass | ✅ Pass | Alto contraste por diseño |

### Screen Readers

Todos los sistemas son compatibles con lectores de pantalla si se usa HTML semántico.

**Consideraciones especiales:**
- **Glassmorphism:** Asegurar que texto tenga contraste suficiente
- **Neumorphism:** Borders sutiles pueden confundir visualmente
- **Claymorphism:** Uso de color no debe ser único indicador

### Keyboard Navigation

Todos los sistemas soportan navegación por teclado si se implementa correctamente.

**Focus states recomendados:**
- **Geist:** Outline azul 2px
- **Glassmorphism:** Outline brillante con backdrop
- **Neumorphism:** Shadow inset + cambio de color
- **Claymorphism:** Border grueso de color contrastante
- **Neo-Brutalism:** Border negro grueso + shadow offset

---

## Casos de Estudio

### Caso 1: SaaS B2B Dashboard
**Selección:** Geist
**Razón:** Accesibilidad crítica, uso prolongado, audiencia técnica, contenido denso.

### Caso 2: App de Música Streaming
**Selección:** Glassmorphism
**Razón:** Fondos visuales (album art), audiencia joven, premium feel, menos contenido textual.

### Caso 3: Portfolio de Diseñador
**Selección:** Neo-Brutalism
**Razón:** Diferenciación visual, creatividad, audiencia creativa, statement de marca.

### Caso 4: App Educativa Infantil
**Selección:** Claymorphism
**Razón:** Alegre, vibrante, engagement, gamificación, audiencia 6-12 años.

### Caso 5: Smart Home Controller
**Selección:** Neumorphism
**Razón:** Controles táctiles, interfaz minimalista, look moderno, app móvil.

---

## Mezcla de Sistemas (Hybrid Approach)

Es posible combinar sistemas en diferentes secciones:

**Ejemplo 1: Landing + Dashboard**
- Landing page: Glassmorphism (impacto visual)
- Dashboard: Geist (funcionalidad y claridad)

**Ejemplo 2: Marketing + App**
- Sitio marketing: Neo-Brutalism (diferenciación)
- App interna: Geist (productividad)

**Consideración:** Mantener consistencia en:
- Tipografía
- Paleta de colores base
- Espaciado/grid
- Componentes compartidos (buttons, forms)

---

## Checklist de Selección

- [ ] ¿Quién es la audiencia principal?
- [ ] ¿Cuáles son los requisitos de accesibilidad?
- [ ] ¿Qué dispositivos soportar?
- [ ] ¿Cuál es el presupuesto de performance?
- [ ] ¿El diseño es para 1 año o 5 años?
- [ ] ¿La marca es conservadora o atrevida?
- [ ] ¿El contenido es denso o espaciado?
- [ ] ¿Hay fondos visuales o sólidos?
- [ ] ¿El cliente tolera riesgos visuales?
- [ ] ¿Hay restricciones de soporte de navegadores?

---

## Recursos de Testing

- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
- [WAVE Accessibility Tool](https://wave.webaim.org/)
- [Lighthouse Accessibility Audit](https://developers.google.com/web/tools/lighthouse)
- [axe DevTools](https://www.deque.com/axe/devtools/)

---

**Última actualización:** 2026-01-20
**Versión:** 1.0
