# 🍲 Olla — Recetas simples

> Recetario de **comida económica** en un solo archivo HTML. Sin servidores, sin cuentas, sin frameworks. Trae recetas precargadas y puedes crear las tuyas. Tus datos viven en tu navegador.

![Tipo](https://img.shields.io/badge/tipo-PWA-ff7043)
![Stack](https://img.shields.io/badge/stack-HTML%20%2B%20CSS%20%2B%20JS%20vanilla-f7df1e)
![Dependencias](https://img.shields.io/badge/dependencias-0-success)
![Plataforma](https://img.shields.io/badge/mobile--first-iOS%20%2F%20Safari-black)

PWA mobile-first pensada para cocinar rico gastando poco. Misma filosofía que su app hermana de finanzas: cero build, datos locales y respaldo opcional a Google Drive.

---

## Características

- **Recetas precargadas** económicas (colombianas): huevos pericos, lentejas, fríjoles, pasta con atún, sopas, arepas…
- **Crear / editar / borrar** tus propias recetas (emoji, ingredientes con cantidad y costo, pasos, porciones, tiempo)
- **Costo por receta y por porción** — el enfoque de bajo presupuesto
- **Lista de mercado**: eliges varias recetas y arma la lista de ingredientes combinada con costo total
- **¿Qué cocino con lo que tengo?**: marcas los ingredientes de tu cocina y te sugiere recetas por % de coincidencia
- **Favoritos, búsqueda y filtros** por categoría
- **Respaldo**: exportar/importar JSON + Google Drive (opcional)

---

## Arquitectura

```
index.html  ← TODO: HTML + CSS + JS vanilla en un solo archivo
│
└── localStorage (claves con prefijo "olla_")
    ├── olla_recipes    → todas las recetas (precargadas + tuyas)
    ├── olla_pantry     → ingredientes que tienes en casa
    ├── olla_drive_cid  → Client ID de Google OAuth (opcional)
    └── olla_drive_fid  → ID del archivo de respaldo en Drive
```

**Privacidad:** el código es genérico (las recetas precargadas no son datos personales). Lo que crees vive solo en tu navegador y, si quieres, en *tu* Google Drive (scope `drive.file`).

**Seguridad:** todo texto que escribes se escapa antes de mostrarse (anti-XSS) y un Content-Security-Policy bloquea el envío de datos a dominios externos.

---

## Despliegue

Hosting estático en **Cloudflare Pages**. Conecta el repo en *Workers & Pages → Create → Pages → Connect to Git*, **Framework preset: None**, **sin build command** y con **build output directory en `/`**. Cada `git push` a `main` redespliega solo.

> ⚠️ No agregar `wrangler.toml`: hace que Cloudflare trate el repo como un Worker (`wrangler deploy`) y el build falla. Es un sitio estático puro.
>
> Sirve igual en cualquier hosting estático (Vercel, GitHub Pages): importar el repo, sin build, publicar la raíz.

Para Google Drive: crea un Client ID OAuth (cuenta personal, consentimiento Externo, Drive API habilitada) y pégalo en Más → Conectar Google Drive. La URL de la app debe estar en "Orígenes de JavaScript autorizados".

---

## Instalación en iPhone

Safari → abrir la URL → Compartir → **Agregar a pantalla de inicio**.

---

## Stack

- HTML + CSS + JavaScript vanilla (cero dependencias de build)
- Google Identity Services + Google Drive API v3 (solo para el respaldo opcional)

## Licencia

Uso personal. Las recetas precargadas son genéricas; cada usuario crea y guarda las suyas.
