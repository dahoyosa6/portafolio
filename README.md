# Handoff — Portafolio David Hoyos · Candidatura 30X Tech Volunteer

> Paquete de entrega para implementar / configurar este portafolio desde **Claude Code**.
> Incluye todo el código, los assets y la documentación necesaria para correrlo tal cual,
> editarlo, desplegarlo o reconstruirlo en otro framework.

---

## 1. Overview

Portafolio personal de **David Hoyos** (ingeniero mecánico convertido en builder de productos
con IA) para aplicar a **30X Tech Volunteer**. Es un sitio editorial, minimalista y **bilingüe
(ES/EN con toggle)** que presenta 5 proyectos, una historia personal, principios de trabajo y un
stack técnico.

- **Página principal:** `Portafolio.dc.html`
- **Dos casos de estudio profundos:** `Saku.dc.html`, `FinanzasIQ.dc.html` (enlazados desde las
  cards destacadas de la principal).
- Estilo: editorial tipo revista — papel hueso, tinta casi negra, un acento terracota. Tipografía
  Instrument Serif (display) + Hanken Grotesk (texto) + JetBrains Mono (etiquetas/datos).

**Objetivo #1 del producto:** que cualquiera pueda **entrar y ver los proyectos en vivo**
(links discretos pero claros en cada card).

---

## 2. Sobre los archivos de diseño (LÉASE PRIMERO)

Los `.dc.html` de este bundle son **"Design Components"**: archivos HTML autocontenidos que
**ya funcionan como un sitio estático** (se abren directo en el navegador). No son un wireframe:
son **hi-fi y funcionales** (colores, tipografía, espaciado e interacciones finales).

Tienes dos caminos válidos con este paquete:

- **(A) Usarlo tal cual** — desplegarlo como sitio estático (es lo más rápido; ver §4). El diseño
  ya está terminado.
- **(B) Reconstruirlo** en el entorno de un codebase existente (React/Next, Vue, etc.) usando sus
  patrones y librerías. Para eso, este README documenta tokens, estructura y comportamiento con
  precisión (ver §6–§10).

**Fidelidad: HI-FI.** Si reconstruyes, replica pixel-perfect con los valores de §8.

---

## 3. Estructura del bundle

```
design_handoff_portafolio_30x/
├── README.md                ← este archivo
├── Portafolio.dc.html       ← página principal (hero, proyectos, historia, cómo trabajo, stack, contacto)
├── Saku.dc.html             ← caso de estudio profundo (agente de ventas IA)
├── FinanzasIQ.dc.html       ← caso de estudio profundo (sistema de finanzas)
├── support.js               ← runtime de los Design Components (NO editar)
└── assets/
    ├── cover-saku.png        ← portada Saku (composición: chat + card de marca)
    ├── cover-finanzasiq.png  ← portada FinanzasIQ (captura del dashboard)
    ├── cover-momentum.png    ← portada Momentum (captura)
    ├── cover-nexo.png        ← portada Nexo (captura)
    ├── cover-askdb.png       ← portada AskDB (mock diseñado, no es captura real)
    ├── CV_David_Hoyos_ES.pdf ← CV en español (enlazado en ES)
    └── CV_David_Hoyos_EN.pdf ← CV en inglés (enlazado en EN)
```

---

## 4. Cómo correr y desplegar (camino A)

Es un sitio 100% estático. Tres rutas para los nombres de archivo con espacios — todos los
enlaces internos ya usan rutas relativas (`Portafolio.dc.html`, `assets/...`).

**Local:**
```bash
cd design_handoff_portafolio_30x
python3 -m http.server 8000
# abrir http://localhost:8000/Portafolio.dc.html
```

**Deploy (Vercel / Netlify / Cloudflare Pages / GitHub Pages):**
- Sube la carpeta completa como sitio estático. No hay build step.
- Sugerencia: renombrar `Portafolio.dc.html` → `index.html` para que sea la raíz del dominio.
  Si lo haces, actualiza los enlaces "Ver caso" en la lógica (ver §7) y el botón "volver" de los
  casos (`href="Portafolio.dc.html"` → `href="index.html"`).
- `support.js` debe quedar junto a los `.html`. Las fuentes se cargan desde Google Fonts (requiere
  internet); si necesitas offline, self-hostea las fuentes.

> Nota técnica: cada `.dc.html` carga `support.js`, que monta el template (entre `<x-dc>…</x-dc>`)
> con su clase de lógica (`class Component extends DCLogic`). El runtime resuelve los `{{ holes }}`
> y `<sc-for>/<sc-if>`. **No edites `support.js`.**

---

## 5. Anatomía de un Design Component

Cada `.dc.html` tiene 3 partes:

1. **`<helmet>`** — fuentes (Google Fonts), resets de `body`, `@keyframes`. (Solo va aquí lo que no
   puede ser inline.)
2. **Template** (`<x-dc>…</x-dc>`) — markup con **estilos inline** y "holes" `{{ valor }}`.
   - `<sc-for list="{{ arr }}" as="item">…</sc-for>` repite por cada elemento (`item.campo`).
   - `<sc-if value="{{ bool }}">…</sc-if>` renderiza condicionalmente.
3. **Clase de lógica** (`<script data-dc-script>`) — `class Component extends DCLogic` con
   `state` y `renderVals()`, que devuelve TODOS los valores que el template consume por nombre.

El **toggle ES/EN** vive en `state.lang` (`'es' | 'en'`); `renderVals()` arma todos los textos con
un helper `p(es, en)` que elige según el idioma activo. Por eso casi todo el texto va por `{{ holes }}`.

---

## 6. Secciones de la página principal (`Portafolio.dc.html`)

Orden vertical:

1. **Header / nav** (sticky): wordmark "DAVID HOYOS / builder" · links (Proyectos, Historia, Stack,
   Contacto) · link "CV (PDF)" · toggle **ES / EN**. Blur de fondo, borde inferior 1px.
2. **Hero** (`#top`): kicker mono ("Candidato · 30X Tech Volunteer" + "Portafolio · 2026 — Medellín"),
   titular display "Del torque *a los tokens*" (segunda mitad en itálica terracota), subtítulo,
   párrafo de propuesta de valor, y fila de links (GitHub, LinkedIn, Email, CV).
3. **Proyectos** (`#proyectos`): título "Cinco cosas que construí y envié." + nota. Luego:
   - **2 cards destacadas** (grid, aspect 16:10): Saku y FinanzasIQ → enlazan a sus casos.
   - **3 cards** (grid, aspect 4:3): Momentum, Nexo, AskDB → enlazan a links en vivo / repo.
   - Cada card: portada (imagen), categoría (mono), año, título/sub (serif), one-liner, y pie con
     **estado** (punto verde lleno = activo / aro hueco = pre-piloto/repo) + label de link.
4. **Historia** (`#historia`): fondo oscuro (#1B1A17). Kicker + título "Cómo llegué aquí" + nota
   lateral + 3 párrafos (lead en itálica serif + cuerpo).
5. **Cómo trabajo** (`#trabajo`): 3 principios numerados (01/02/03), cada uno con borde superior,
   título serif y descripción.
6. **Stack** (`#stack`): grid de 5 grupos (IA aplicada, Automatización, Datos, Producto/Front,
   Infra & seguridad), cada uno con su lista de herramientas. Rejilla con líneas de 1px.
7. **Contacto** (`#contacto`): fondo oscuro. Titular gigante "¿Construimos algo?", email grande
   enlazado, fila de links (email, tel, GitHub, LinkedIn) y pie con copyright.

---

## 7. Cómo editar lo más común

Todo el contenido editable vive en `renderVals()` de cada archivo (busca el bloque
`<script data-dc-script>`).

### Proyectos (array `RAW` en `Portafolio.dc.html`)
Cada proyecto es un objeto:
```js
{ num:'03', name:'Momentum',
  subEs:'Productividad', subEn:'Productivity',
  catEs:'SaaS · PWA · Productividad', catEn:'SaaS · PWA · Productivity',
  olEs:'…one-liner ES…', olEn:'…one-liner EN…',
  stEs:'En vivo', stEn:'Live',           // texto de estado
  glyph:'PWA · PLAN VS. REAL',           // etiqueta de portada (fallback sin imagen)
  active:true,                            // true = punto verde lleno; false = aro hueco
  cover:'assets/cover-momentum.png',      // portada (o null → usa title-card tipográfica)
  pos:'center',                           // object-position de la portada (encuadre)
  href:'https://momentum-plum-seven.vercel.app',
  external:true,                          // true → abre en pestaña nueva (links en vivo/repo)
  llEs:'Ver en vivo', llEn:'View live',   // label del link
  year:'2026' }
```
- Las **2 primeras** entradas del array son las destacadas (16:10); el resto son 4:3
  (`projects.slice(0,2)` vs `slice(2)` al final de `renderVals`).
- Para **cambiar una portada**: reemplaza el PNG en `assets/` y, si hace falta, ajusta `pos`
  (`'center'`, `'left top'`, `'center top'`…) para reencuadrar (es `object-position` del `<img>`).
- Las imágenes de portada se montan como `coverEl` (un `React.createElement('img', …)`) para que el
  `src` no falle durante el streaming — si agregas un proyecto con imagen, el `.map()` ya lo arma.

### Textos generales
Objeto `t = { … }` dentro de `renderVals()`: cada clave usa `p('texto ES','texto EN')`. Edita el
par y listo.

### Color de acento
El terracota es `#BC4B2E` (claro) / `#D98B6E` (sobre fondos oscuros). Está inline en muchos sitios;
para cambiarlo globalmente, busca y reemplaza esos dos hex.

### Idioma por defecto
`state = { lang: 'es' }` al inicio de la clase. Cambia a `'en'` si quieres arrancar en inglés.

### Casos de estudio (`Saku.dc.html`, `FinanzasIQ.dc.html`)
Misma estructura. El contenido (métricas, problema, solución, decisión clave, stack, aprendizaje,
estado/repo) está en `renderVals()` con el mismo patrón `p(es,en)`. Los arrays `metrics`, `stack`,
`decisions` (FinanzasIQ) controlan las rejillas.

---

## 8. Design tokens

### Colores
| Token | Hex | Uso |
|---|---|---|
| Papel (fondo) | `#F5F2EC` | fondo principal claro |
| Papel card/portada | `#ECE7DC` | fondo de cards sin imagen |
| Tinta | `#1B1A17` | texto principal / fondos oscuros (Historia, Contacto) |
| Tinta 2 | `#33302B` / `#403D37` / `#56524B` | jerarquía de texto |
| Muted | `#79746A` / `#9C968A` | metadatos, mono labels |
| Línea / borde | `rgba(27,26,23,0.10–0.18)` | bordes y divisores |
| Acento (claro) | `#BC4B2E` | links, kickers, acentos, itálica del hero |
| Acento (oscuro) | `#D98B6E` | acento sobre fondos #1B1A17 |
| Verde "en vivo" | `#2E7D52` | punto de estado activo |
| Texto sobre oscuro | `#EDE9E0` / `#CFC9BD` / `#B7B1A5` | en secciones oscuras |

**Marca Saku** (solo en `cover-saku.png` y dentro del caso): granate `#71203A`, ámbar `#E0A458`,
verde `#0B6B53`, tinta `#1B1410`, papel `#F6F4EF`.

### Tipografía
| Rol | Familia | Uso |
|---|---|---|
| Display | **Instrument Serif** (400, e itálica) | titulares, nombres de proyecto, leads |
| Texto / UI | **Hanken Grotesk** (400–700) | cuerpo, descripciones |
| Mono | **JetBrains Mono** (400–500) | kickers, categorías, estado, métricas, nav |

Tamaños clave (clamp responsive): hero `clamp(58px,11.5vw,148px)`; títulos de sección
`clamp(34px,5vw,56px)`; titular de contacto `clamp(48px,9vw,116px)`; cuerpo 15–19px; mono labels
11–13px con `letter-spacing` 0.04–0.08em y `text-transform:uppercase` en kickers.

### Espaciado y layout
- Contenedor principal: `max-width:1120px`, padding lateral `28px`.
- Contenedor de casos: `max-width:920px`.
- Padding vertical de secciones: `clamp(56px,9vw,104px)`.
- Grids de cards: `repeat(auto-fit, minmax(330px,1fr))` (destacadas) y `minmax(280px,1fr))` (resto),
  gap `clamp(20px,3vw,40px)`.
- Radios: cards/portadas `3px`; toggle/botones `6–8px`; pills `999px`; callouts de caso `4px`.
- Bordes: 1px. Líneas de sección: `1px solid rgba(27,26,23,0.12)`.

### Estado (badges)
- Activo / en producción / en vivo → punto **lleno** `#2E7D52`.
- Pre-piloto / repo / por desplegar → **aro hueco** `1.5px solid #9C968A`.

---

## 9. Interacciones y comportamiento

- **Toggle ES/EN:** botones `ES`/`EN` en el nav → `setLang('es'|'en')`. Re-renderiza todos los
  textos. El botón activo va con fondo tinta `#1B1A17` y texto papel; el inactivo, transparente.
  El link "CV (PDF)" cambia entre `CV_David_Hoyos_ES.pdf` y `…_EN.pdf` según idioma.
- **Nav links:** anchors a `#proyectos`, `#historia`, `#stack`, `#contacto` con `scroll-behavior:smooth`
  y `scroll-padding-top` para compensar el header sticky. En `<760px` se ocultan los links centrales
  (media query en `<helmet>`).
- **Hover:** cards → el borde de la portada se oscurece; links → aparece subrayado; nav links →
  fondo sutil. Todo con `transition` 0.15–0.2s.
- **Links de proyecto:** los `external:true` abren en pestaña nueva (`target="_blank"`,
  `rel="noopener noreferrer"`); los casos (Saku/FinanzasIQ) navegan en la misma pestaña.
- **Punto "en línea" del hero:** animación `pulseDot` (2.4s).
- **Casos:** header con "← Portafolio" + toggle; al final, navegación a siguiente/anterior caso y
  "Todos los proyectos".

---

## 10. Datos en vivo y REGLAS DE HONESTIDAD (no alterar sin verificar)

Estos datos son intencionalmente precisos y conservadores. **No inflar.** Lo no medido va marcado
como estimación/proyección.

- **Saku** — estado **pre-piloto / pruebas internas** (NO "en producción" ni "piloto"). Evals
  **~80%** sobre **84 conversaciones**. Catálogo **860 SKUs**. Costo/turno **~US$0.022**.
  **ROI ~21× = proyección/modelo** (no medido). Cliente sin nombrar (PyME de pinturas en Toluca, MX).
  Sin Docker. Repo a publicar **sanitizado**.
- **FinanzasIQ** — **en producción**, uso diario desde mayo 2026. 16 cuentas, 54 categorías.
  "Cero registro manual **en el flujo principal**" (matiz, no absoluto). Repo privado.
- **Momentum** — **en vivo**: https://momentum-plum-seven.vercel.app · 118 tests.
- **Nexo** — **en vivo**: https://nexo-opal-sigma.vercel.app · 334 becas / 8 fuentes · 543 tests ·
  robot de datos en producción (GitHub Actions).
- **AskDB** — repo público https://github.com/dahoyosa6/askdb · 211 tests · estado **deploy en
  proceso** (no presentarlo como funcional en vivo todavía). La portada (`cover-askdb.png`) es un
  **mock diseñado** (datos ilustrativos), no una captura real.

**Contacto:** dahoyosa6@gmail.com · +57 320 315 4133 · Medellín, CO · github.com/dahoyosa6 ·
linkedin.com/in/davidhoyosalzate.

---

## 11. Assets

| Archivo | Origen | Notas |
|---|---|---|
| `cover-saku.png` | Composición HTML (chat de WhatsApp reconstruido en HTML + card de marca "El sistema, de un vistazo") | 16:10. Marca Saku (granate/ámbar). |
| `cover-finanzasiq.png` | Captura real del dashboard | Dashboard oscuro "Resumen Financiero". |
| `cover-momentum.png` | Captura real | "Plan vs. realidad", encuadre centrado. |
| `cover-nexo.png` | Captura real | Lista "Tus oportunidades". |
| `cover-askdb.png` | **Mock diseñado** (no captura) | 4:3. Chat Telegram → datos, datos ilustrativos. |
| `CV_David_Hoyos_ES.pdf` / `_EN.pdf` | CVs finales | Enlazados según idioma. |

Fuentes: Google Fonts (Instrument Serif, Hanken Grotesk, JetBrains Mono). En los casos y en la
portada de Saku se usan también Bricolage Grotesque + Inter (marca Saku).

---

## 12. Si reconstruyes en un framework (camino B)

- **Recomendado:** Next.js + TypeScript (encaja con el resto del stack de David). Una página por
  ruta: `/` (principal), `/casos/saku`, `/casos/finanzasiq`.
- Modela el contenido como datos (un `projects.ts`, un diccionario `i18n` ES/EN) y renderiza con
  componentes. El toggle de idioma = un estado/route (`/en`). 
- Replica los tokens de §8 (idealmente como CSS variables o theme). Conserva estilos inline → puedes
  migrarlos a Tailwind o CSS Modules manteniendo los valores exactos.
- Mantén las **reglas de honestidad** (§10) en el contenido. Mantén los links en vivo y el objetivo
  de "que se pueda entrar a ver los proyectos".
- Las portadas son imágenes; si quieres, la de Saku y la de AskDB pueden reconstruirse como
  componentes (su markup HTML está descrito en este repo / fue generado a partir de HTML).

---

## 13. Checklist rápido para Claude Code

- [ ] Correr local (§4) y verificar las 3 páginas + toggle ES/EN.
- [ ] Decidir camino A (deploy estático) o B (reconstrucción).
- [ ] Si A: renombrar `Portafolio.dc.html`→`index.html` y ajustar enlaces (§4).
- [ ] Verificar que los links en vivo abren (Momentum, Nexo) y los CVs descargan.
- [ ] No alterar datos sin verificar (§10).
