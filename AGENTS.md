# AGENTS.md

## Propósito
Guía de arquitectura y contribución para el proyecto `ai-timeline`. **Es de lectura obligatoria antes de hacer cualquier cambio.** Las reglas aquí descritas no son sugerencias: son contratos que preservan la integridad técnica del proyecto.

---

## Principio fundamental: arquitectura zero-dependency

Este proyecto es un **único archivo HTML autocontenible** (`ai-timeline.html`). Esa restricción es una decisión de diseño deliberada, no una limitación temporal. Significa:

- Sin `npm`, `yarn`, `pnpm` ni ningún gestor de paquetes.
- Sin `node_modules`, `package.json` ni `package-lock.json`.
- Sin bundlers (Webpack, Vite, Rollup, Parcel…).
- Sin frameworks JS (React, Vue, Angular, Svelte…).
- Sin librerías CSS (Tailwind, Bootstrap, Material…).
- Sin preprocesadores (Sass, Less, PostCSS…).
- Sin TypeScript ni ningún paso de compilación.
- Sin CDN externo: ningún `<script src="https://...">` ni `<link href="https://...">`.
- Sin fuentes externas: `font-family` usa únicamente fuentes del sistema (`system-ui`).

**Cualquier PR que introduzca una dependencia externa será rechazado sin revisión.**

---

## Estructura del repositorio

```
.
├── ai-timeline.html   # Aplicación completa — HTML + CSS + JS en un solo archivo
├── AGENTS.md          # Este archivo
└── README.md          # Documentación pública del proyecto
```

No se deben crear carpetas, archivos de configuración ni scripts auxiliares sin justificación explícita y consenso previo.

---

## Arquitectura interna de `ai-timeline.html`

El archivo tiene tres secciones bien delimitadas. **No alterar su orden.**

### 1. `<style>` — Sistema de diseño

Todos los colores y espaciados se definen como CSS custom properties en `:root`. **No usar valores hardcoded** fuera de los ya existentes.

```css
:root {
  --bg:      #0a0e1a;   /* fondo de página */
  --surface: #111827;   /* fondo de tarjetas y widgets */
  --border:  #1e293b;   /* bordes */
  --accent1: #6366f1;   /* índigo — primario */
  --accent2: #22d3ee;   /* cian — secundario */
  --accent3: #f472b6;   /* rosa — acento terciario */
  --text:    #e2e8f0;   /* texto principal */
  --muted:   #64748b;   /* texto secundario */
  --line:    2px;       /* grosor de la línea vertical del timeline */
}
```

Colores de categoría (hardcoded junto a su uso, no en `:root`):

| Clase CSS       | Color     | Uso                                     |
|-----------------|-----------|-----------------------------------------|
| `cat-research`  | `#818cf8` | Papers, algoritmos, avances teóricos    |
| `cat-hardware`  | `#22d3ee` | Hardware, datasets, infraestructura     |
| `cat-milestone` | `#f472b6` | Hitos históricos o sociales             |
| `cat-model`     | `#34d399` | Arquitecturas y lanzamientos de modelos |
| `cat-product`   | `#fbbf24` | Productos y lanzamientos comerciales    |

El breakpoint responsivo es `600px`. El layout pasa de doble columna a columna única.

### 2. `<body>` — Marcado HTML

Orden fijo de elementos en el `<body>` — **no reordenar**:

1. `<header>` — título, subtítulo y contador de origen.
2. `.legend` — leyenda de categorías.
3. `.filters` — botones de filtro por década.
4. `#timeline` — bloques de décadas y tarjetas de eventos.
5. `<footer>` — pie de página.

### 3. `<script>` — Lógica JavaScript

Tres módulos en orden fijo — **no reordenar**:

1. **Contador de origen** — IIFE con `setInterval(tick, 1000)`. Origen: `1956-06-18T09:00:00Z`.
2. **Animación de scroll** — `IntersectionObserver` sobre `.event` (umbral `0.15`).
3. **Filtro por décadas** — event listeners sobre `.filters button`, filtra por `data-decade`.

El JS es ES6+ vanilla. No añadir transpilación ni polyfills.

---

## Anatomía de una tarjeta de evento

Estructura canónica obligatoria para cada evento:

```html
<div class="event cat-model" data-decade="2020">
  <div class="dot"></div><div class="spacer"></div>
  <div class="card">
    <div class="card-year">2020</div>
    <div class="card-title">Título del evento</div>
    <div class="card-desc">Descripción concisa del hito (2-3 frases).</div>
    <a class="card-link"
       href="https://es.wikipedia.org/wiki/..."
       target="_blank"
       rel="noopener noreferrer">Leer más ↗</a>
    <span class="tag cat-model">Modelo</span>
  </div>
</div>
```

### Reglas al añadir un evento

1. Colocar dentro del bloque `<!-- ══ 19XXs ══ -->` correcto.
2. `data-decade` = año de apertura de la década (ej. `"1990"` para 1995).
3. La clase de categoría debe ser la misma en `.event` y en `.tag`.
4. El `.card-link` es **obligatorio**. `href` debe ser una URL real — no `#`, no vacío.
5. Preferir Wikipedia en español o, para papers, `arxiv.org` o la publicación original.
6. Siempre usar `target="_blank" rel="noopener noreferrer"` en enlaces externos.
7. Al añadir una nueva década, el botón `.filters` y el `<span>` de `.decade-label` usan el formato `1950's` (apóstrofo antes de `s`). Los atributos `data-filter` y `data-decade` son solo el año numérico (`"1950"`).
8. Añadir el botón de filtro en `.filters` en orden cronológico.

---

## Lo que está terminantemente prohibido

```
✗  <script src="https://...">  o  <link href="https://...">
✗  @import url('...')  dentro del <style>
✗  npm install / yarn add / cualquier gestor de paquetes
✗  import ... from '...'  (módulos ES o CommonJS externos)
✗  fetch() o XHR para cargar datos (los datos van hardcoded en el HTML)
✗  Frameworks JS o librerías CSS de cualquier tipo
✗  document.write() / eval() / new Function()
✗  Inline handlers: onclick="...", onload="..."
✗  IDs duplicados en el DOM
✗  href="#" o href="" en .card-link
✗  target="_blank" sin rel="noopener noreferrer"
✗  console.log() en el código final
✗  var  (usar const / let)
```

---

## Convenciones de código

- **CSS**: propiedades ordenadas por categoría (posicionamiento → caja → tipografía → visual → transición). Secciones marcadas con `/* ── Nombre ── */`.
- **HTML**: indentación de 2 espacios. Sin atributos booleanos con valor explícito.
- **JS**: `const`/`let`. Funciones nombradas sobre anónimas en lógica compleja.
- **Idioma**: interfaz y documentación en español. Comentarios de código en inglés.

---

## Validación antes de cualquier PR

```bash
python3 -m http.server 8080
# Abrir http://localhost:8080/ai-timeline.html
```

Checklist obligatorio:

- El contador actualiza los segundos en tiempo real.
- El filtro "Todos" muestra los 30 eventos; cada botón de década filtra correctamente.
- Cada tarjeta muestra "Leer más ↗" y el enlace abre en pestaña nueva.
- Layout correcto en viewport < 600 px (columna única).
- Sin errores en consola del navegador (`F12 → Console`).
- Sin peticiones de red externas en `Network` (solo el propio HTML).

---

## Historial de versiones

- **v1** — Estructura inicial con eventos desde 1980's hasta 2025.
- **v2** — Cobertura ampliada desde 1956; décadas 1950's, 1960's y 1970's añadidas.
- **v3** — Contador en tiempo real desde el 18 jun 1956 con enlace a Wikipedia.
- **v4** — Enlace `Leer más ↗` en cada tarjeta de evento.
- **v5** — Formato de décadas actualizado a `1950's` (apóstrofo antes de la `s`).
- **v6** — AGENTS.md reescrito con contratos de arquitectura explícitos y checklist de validación.
