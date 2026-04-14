# Mexas Gaming — Clan MxG

Sitio web oficial del clan **Mexas Gaming (MxG)**. Hecho en HTML/CSS/JS puro, sin frameworks ni dependencias externas. Todo en un solo archivo `Index.html`.

## Archivos del proyecto

| Archivo | Descripción |
|---|---|
| `Index.html` | Sitio principal activo — versión de producción |
| `index-1.html` | Versión alternativa / experimental ("Final 10") con integración cloud |
| `CLAUDE.md` | Este archivo — guía para Claude Code |

## Estructura del sitio (`Index.html`)

El sitio tiene estas secciones en orden:

1. **Intro / Loading screen** — animación de bienvenida con barra de progreso
2. **Nav** — menú fijo con scroll activo
3. **Hero** — imagen de portada, botón de ingreso al clan
4. **About `#about`** — historia y valores del clan
5. **Events `#events`** — sistema de eventos (CRUD con modal)
6. **Points `#points`** — tabla de puntos de miembros
7. **Members `#members`** — equipo/roster del clan
8. **Live `#live`** — embed de Twitch (lazy-loaded)
9. **Social** — links sociales
10. **Footer** — logo y links

## Sistema de administración

- Contraseña admin: `MXG2026`
- Sesión de 10 minutos tras autenticarse (`isAuthenticated = true`)
- Acciones protegidas: agregar evento, editar evento, borrar evento
- Los cambios **no persisten** al recargar — viven solo en memoria DOM

## Canal Twitch

```js
const twitchChannel = 'mexasgaming';
```

El embed se carga en `loadTwitchEmbed()` solo cuando la sección entra al viewport (IntersectionObserver).

## Paleta de colores

```css
--green:  #4AE54A   /* verde clan */
--dark:   #060a0d   /* fondo principal */
--dark2:  #0b1118
--dark3:  #111920
--dark4:  #18242e
--white:  #fff
--gray:   #7a8fa3
--border: #1a2b38
```

## Fuentes

- `Bebas Neue` — títulos grandes
- `Barlow Condensed` — subtítulos, UI
- `Barlow` — cuerpo de texto

## Links sociales activos

- WhatsApp: grupo del clan
- Twitch: `twitch.tv/mexasgaming`
- Facebook: grupo de MxG

## Integración Cloud (objetivo de esta rama)

La rama `claude/setup-cloud-integration-Hp6Kt` tiene como objetivo hacer que los datos del sitio sean **persistentes**, jalándolos desde la nube en lugar de estar hardcodeados en el HTML.

### Datos a persistir en cloud

| Sección | Datos |
|---|---|
| Eventos | título, descripción, fecha, estado (soon / active / season) |
| Miembros | nombre, rol, puntos, avatar |
| Tabla de puntos | ranking, puntos por jugador |

### Enfoque recomendado: GitHub como backend

Usar **raw.githubusercontent.com** para servir archivos JSON desde el mismo repositorio. Sin costo, sin backend, sin auth extra.

```
GET https://raw.githubusercontent.com/myhabid-code/MXG/main/data/events.json
GET https://raw.githubusercontent.com/myhabid-code/MXG/main/data/members.json
```

Para **escribir** datos (crear/editar/borrar), usar la **GitHub Contents API** desde el cliente con un token de solo escritura en ese repo.

### Patrón de fetch para el sitio

```js
const RAW_BASE = 'https://raw.githubusercontent.com/myhabid-code/MXG/main/data';

async function loadEvents() {
  const res = await fetch(`${RAW_BASE}/events.json`);
  const events = await res.json();
  renderEvents(events);
}
```

## Notas de desarrollo

- Las imágenes de fondo están **embebidas como base64** dentro del HTML — no hay archivos externos de imagen.
- El sitio funciona sin servidor (puede abrirse como archivo local).
- El admin password está en texto plano en el JS del cliente — es intencional para uso interno del clan.
- No hay bundler, linter ni build step. Editar directamente el HTML.

## Comandos útiles

```bash
# Ver el sitio localmente
python3 -m http.server 8080
# luego abrir http://localhost:8080/Index.html

# Subir cambios
git add .
git commit -m "descripción del cambio"
git push -u origin claude/setup-cloud-integration-Hp6Kt
```
