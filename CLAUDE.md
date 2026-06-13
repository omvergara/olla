# CLAUDE.md — Contexto del proyecto Olla

## Qué es esto
PWA de **recetas económicas** en UN SOLO archivo (`index.html`). HTML + CSS + JS vanilla, cero build, cero dependencias. App hermana de "Cartera" (finanzas), misma filosofía y autor (Mauricio Vergara).

- **Repo:** https://github.com/omvergara/olla (público)
- **Deploy:** Cloudflare Pages (auto-deploy en push a main; config en `wrangler.toml`)

## Arquitectura crítica
- El código es 100% genérico — las recetas precargadas (`DEFAULT_RECIPES`) son contenido genérico, NO datos personales.
- Datos del usuario en `localStorage` con prefijo `olla_`.
- Respaldo opcional en Google Drive del usuario (scope `drive.file`).

**Claves localStorage (no renombrar sin migración):**
```
olla_recipes    array de recetas {id,emoji,name,cat,serv,min,fav,ing:[{n,q,c}],steps:[]}
olla_pantry     array de nombres de ingredientes que el usuario tiene en casa
olla_drive_cid  Client ID OAuth
olla_drive_fid  file ID del respaldo en Drive
```
Las recetas precargadas tienen id `r_*`; las creadas por el usuario `u_*`.

## Reglas de oro
1. **NUNCA** hardcodear datos personales. Las recetas precargadas son genéricas.
2. **Un solo archivo.** No fragmentar salvo decisión explícita del dueño.
3. **Seguridad primero:** todo dato del usuario pasa por `esc()` antes de ir a `innerHTML`. Hay un CSP en el `<head>`.
4. Todo cambio que escriba en localStorage llama `autoSync()` (sube a Drive si hay token).
5. Tras restaurar/importar respaldo: usar `applyBackupObject()` + `location.reload()` (no solo `renderAll()`).
6. Español colombiano. Moneda: `toLocaleString('es-CO')` con `$`.
7. Mobile-first (iPhone, ~390px). Confirmaciones con `showConfirm()`.

## Convenciones del código
- Render por pantalla: `rRecetas()`, `rCocinar()`, `rMercado()`, `rMas()`. `renderAll()` redibuja la activa.
- Navegación: `goTo(screen)`. Modales/overlays: `openOv(id)`/`closeOv(id)`.
- Costo: `recipeCost(r)` (total) y `recipeCostServ(r)` (por porción).
- FAB: se crea en `rRecetas()` y se elimina al cambiar de pantalla.

## Testing manual mínimo
1. Abrir → aparecen 12 recetas precargadas
2. Abrir una receta → ingredientes, pasos, costo total y por porción
3. Crear receta nueva → aparece arriba en la lista
4. Cocinar → marcar ingredientes → ver recetas por % de match
5. Mercado → elegir 2 recetas → lista combinada + costo total
6. Exportar JSON → importar → datos intactos (recarga sola)
7. Sin errores en consola

## Backlog
- Escalar cantidades por porciones (ej: "para 6 personas")
- Modo oscuro (ya implementado en la app hermana Cartera, se puede portar)
- Foto real de receta (hoy solo emoji)
- Categorías/etiquetas extra (vegetariano, rápido)
- Tests automatizados (ver `test/smoke.mjs` de Cartera como base)
