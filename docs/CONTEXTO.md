# PROYECTO TACHOATOMICO

Sitio estático (HTML/CSS/JS) del artista **TACHOATOMICO** (José Ricardo Moreno Chejab).

- **Repositorio GitHub**: `tachoatomico/portal` (rama `main`)
- **Despliegue**: Coolify en VPS `207.180.242.253` → `tachoatomico.poordesigner.com`
- **Media** (imágenes y videos) alojados en el propio repositorio (carpetas `imagenes/` y `videos/`)

---

## Estructura del proyecto

```
/
├── index.html              # Página principal (menú: bio, arte, expo, videos)
├── tachoatomico.html       # Duplicado de la principal (legado, no usar)
├── cv.html                 # CV / Hoja de vida
├── portafolio.html         # Portafolio completo (declaración + arte + exposiciones, printable)
│
├── bio.html                # Página interior — biografía
├── arte.html               # Página interior — obras agrupadas por serie
├── expo.html               # Página interior — exposiciones
├── videos.html             # Página interior — reproductores de video por obra
│
├── content.js              # TODO el texto y datos centralizados (único lugar para editar)
├── components.js           # Funciones compartidas (headers, navs, renders)
│
├── imagenes/               # Imágenes (obras, exposiciones, logo)
├── videos/                 # Videos .mp4 (uno o varios por obra)
│
├── fonts/
│   └── Moonlite Solid.otf  # Tipografía de títulos y menús
│
├── hojadevida/
│   └── 2025 hv tachoatomico.pdf  # PDF original escaneado
│
└── docs/
    ├── CONTEXTO.md         # Este archivo
    └── contexto_coolify/   # Datos del servidor y flujo de deploy
```

---

## Arquitectura del sistema

Todas las páginas cargan `content.js` y `components.js` y no tienen texto hardcodeado: el contenido vive en `content.js` y el render en `components.js`.

### content.js
Objeto global `CONTENIDO` con estas secciones:

| Campo | Descripción |
|-------|-------------|
| `grupos_texto` | Texto transversal que aparece debajo del título de cada grupo/serie |
| `bio` | Array de párrafos de la biografía |
| `perfil` | Texto del perfil profesional (cv.html) |
| `academico` | `{year, role, place}` |
| `arte_trayectoria` | `{year, role, desc}` |
| `laboral` | `{year, role, place}` |
| `proyectos` | Proyectos del portafolio (cv.html) |
| `exposiciones` | Exposiciones para cv.html |
| `obras` | Obras de arte (arte.html, portafolio.html, videos.html) |
| `expo_list` | Exposiciones (expo.html, portafolio.html) |

### components.js
Funciones de render compartido:

| Función | Uso |
|---------|-----|
| `agregarPuntos(selector, distMax)` | Puntos aleatorios (atom.png) alrededor de menús |
| `renderMainHeader(id)` | Header principal (index, portafolio, cv) |
| `renderInnerHeader(id)` | Header interior (bio, arte, expo, videos) con INICIO + menú |
| `renderObrasAgrupadas(id, renderFn)` | Obras agrupadas por `complemento_titulo` |
| `timelineHTML(items)` | Línea de tiempo |
| `proyectoHTML(p)` | Tarjeta de proyecto |
| `expoCardHTML(e)` | Tarjeta de exposición (cv) |
| `obraHTML(o)` | Bloque completo de obra (arte/portafolio) |
| `expoItemHTML(e)` | Bloque completo de exposición |
| `obraFilaHTML(o)` / `expoFilaHTML(e)` | Layout por filas (legado del portafolio, no usar) |
| `absolutizarLinks()` | Convierte links relativos a absolutos vs `tachoatomico.poordesigner.com` |
| `slugify(s)` | Genera slug para clases CSS |

---

## Formato de obra (`CONTENIDO.obras`)

```js
{
  titulo: "nombre",
  complemento_titulo: "serie o categoría",   // define el grupo
  exposicion: "lugar de exposición (opcional)",
  año: "2026",
  descripcion: "texto descriptivo",
  partes: [          // opcional — sub-partes con su propia imagen/video (ej. KIEP)
    { titulo: "Karma", imagen: "x.png", video: "videos/x.mp4", texto: "..." }
  ],
  imgs: ["archivo1.jpg", "archivo2.jpg"],
  videos: ["videos/archivo.mp4"],
  links: [{ url: "https://...", label: "texto" }]
}
```

- Las obras se **agrupan automáticamente** por `complemento_titulo`.
- El texto del grupo se define en `CONTENIDO.grupos_texto[complemento_titulo]`.

## Formato de exposición (`CONTENIDO.expo_list`)

```js
{
  titulo: "nombre",
  complemento_titulo: "categoría o colectivo",
  lugar: "lugar físico",
  año: "fecha (ej: Nov/2024)",
  descripcion: "texto (separar párrafos con \\n\\n)",
  imgs: ["archivo1.jpg", ...],
  videos: [],
  links: [{ url: "https://...", label: "texto" }]
}
```

---

## Guía: crear una nueva entrada

Cuando digas **"crear nueva entrada"**, preguntar en este orden:

### OBRA (arte.html)
1. título · 2. complemento-titulo (serie/grupo) · 3. exposición (opcional) · 4. año · 5. descripción · 6. partes (opcional) · 7. imágenes (`imagenes/`) · 8. videos (`videos/`) · 9. links

### EXPOSICIÓN (expo.html)
1. título · 2. complemento-titulo · 3. lugar · 4. año/fecha · 5. descripción · 6. imágenes · 7. videos · 8. links

---

## Tipografías y colores

**Tipografías**
- Moonline Solid — títulos, menús, títulos de sección/grupo
- Courier New — tagline y meta-tags del header
- Roboto — cuerpo de texto (bio, arte, expo, portafolio)
- Inter — cuerpo general (cv)

**Colores**
- Fucsia `#ff0066` / `#c00080` — tagline, meta-tags, complemento-titulo, links
- Texto `#555` / `#333` / `#111` — párrafos y títulos
- Bordes `#e0e0e0` — separadores

---

## Despliegue

- Repo: `tachoatomico/portal`, rama `main`
- Coolify hace deploy automático al pushear a `main`
- Credenciales git: PAT de `tachoatomico` guardado en Windows Credential Manager
- Ver `docs/contexto_coolify/` para datos del VPS, SSH y flujo de deploy

## Imprimir / exportar PDF

`portafolio.html` y `cv.html` tienen estilos `@media print` (tamaño carta, paginación por secciones/grupos). Para exportar con links clicables usar **"Guardar como PDF"** de Chrome (no "Imprimir").
