# Nexus Digital — Sitio web de la agencia

Stack: **Astro 4** + **Decap CMS** + **Netlify** (todo gratuito)

---

## Estructura del proyecto

```
mi-agencia/
├── public/
│   ├── admin/              ← Panel CMS (no tocar)
│   │   ├── index.html
│   │   └── config.yml      ← Configuración de campos editables
│   └── images/             ← Imágenes subidas desde el CMS
│
├── src/
│   ├── components/         ← Una sección = un archivo
│   │   ├── Hero.astro          (hero + barra de logos)
│   │   ├── Servicios.astro     (cards de los 3 servicios)
│   │   ├── Proceso.astro       (pasos del proceso)
│   │   ├── Casos.astro         (casos de éxito)
│   │   ├── Testimonios.astro   (testimonios)
│   │   ├── Nosotros.astro      (equipo + historia)
│   │   ├── BlogPreview.astro   (últimos 3 artículos)
│   │   └── Contacto.astro      (formulario)
│   │
│   ├── content/            ← AQUÍ van los textos editables
│   │   ├── servicios/
│   │   │   ├── crm.md
│   │   │   ├── ecommerce.md
│   │   │   └── social-media.md
│   │   ├── casos/
│   │   │   ├── caso-01.md
│   │   │   └── caso-02.md
│   │   └── blog/
│   │       └── primer-articulo.md
│   │
│   ├── data/               ← Textos simples en JSON
│   │   ├── hero.json           (titular, subtítulo, stats, tags)
│   │   ├── proceso.json        (pasos del proceso)
│   │   ├── testimonios.json    (textos y datos de testimonios)
│   │   ├── nosotros.json       (historia, valores, equipo)
│   │   └── contacto.json       (email, WhatsApp, textos)
│   │
│   ├── layouts/
│   │   └── Layout.astro    ← Nav + Footer + estilos globales
│   │
│   └── pages/
│       ├── index.astro         ← Landing principal
│       ├── blog/
│       │   ├── index.astro     ← Listado del blog
│       │   └── [slug].astro    ← Artículo individual
│       └── servicios/
│           └── [slug].astro    ← Página interna de servicio
│
├── astro.config.mjs
├── netlify.toml
└── package.json
```

---

## Setup inicial (una sola vez)

```bash
# 1. Instalar dependencias
npm install

# 2. Levantar en local
npm run dev
# → abre http://localhost:4321

# 3. Build de producción
npm run build
```

---

## Cómo editar contenido

### Opción A — Editar archivos directamente (recomendado mientras desarrollas)

| Quiero cambiar...             | Archivo a editar                          |
|-------------------------------|-------------------------------------------|
| Titular del hero              | `src/data/hero.json`                      |
| Subtítulo, CTA, stats del hero | `src/data/hero.json`                    |
| Pasos del proceso             | `src/data/proceso.json`                   |
| Testimonios                   | `src/data/testimonios.json`               |
| Historia y equipo             | `src/data/nosotros.json`                  |
| Email, WhatsApp, contacto     | `src/data/contacto.json`                  |
| Descripción de un servicio    | `src/content/servicios/crm.md` (o ecommerce/social-media) |
| Caso de éxito                 | `src/content/casos/caso-01.md`            |
| Artículo del blog             | `src/content/blog/nombre-articulo.md`     |

### Opción B — Panel visual (una vez conectado a Netlify)

1. Ve a `https://tu-sitio.netlify.app/admin`
2. Inicia sesión con tu email
3. Edita blog, servicios y casos sin tocar código
4. Guarda → Netlify despliega automáticamente en ~40 segundos

---

## Deploy en Netlify (paso a paso)

### 1. Subir a GitHub
```bash
git init
git add .
git commit -m "Primer commit — sitio agencia"
git remote add origin https://github.com/TU_USUARIO/mi-agencia.git
git push -u origin main
```

### 2. Conectar a Netlify
1. Entra a [netlify.com](https://netlify.com) y crea cuenta gratuita
2. *Add new site* → *Import from Git* → GitHub
3. Selecciona el repositorio `mi-agencia`
4. Build command: `npm run build`
5. Publish directory: `dist`
6. *Deploy site* → listo en ~2 minutos

### 3. Activar el CMS
1. En Netlify → *Site configuration* → *Identity* → **Enable Identity**
2. Baja a *Registration* → selecciona **Invite only**
3. *Services* → *Git Gateway* → **Enable Git Gateway**
4. Ve a *Identity* → *Invite users* → ingresa tu email
5. Revisa tu email y acepta la invitación
6. Ahora puedes entrar a `tu-sitio.netlify.app/admin`

### 4. Activar el formulario de contacto
El formulario ya tiene `data-netlify="true"`. Solo necesitas:
- En Netlify → *Forms* → verás el formulario "contacto" automáticamente
- Las respuestas llegan a tu email y al panel de Netlify

---

## Cómo agregar un nuevo artículo de blog (sin CMS)

Crea un archivo en `src/content/blog/nombre-del-articulo.md`:

```markdown
---
slug: nombre-del-articulo
title: "El título del artículo"
date: 2025-02-01
excerpt: "Breve descripción que aparece en el listado."
category: "CRM"
minutos: 6
imagen: "/images/mi-imagen.jpg"
---

## Introducción

Aquí va el contenido del artículo en Markdown...
```

Guarda, haz commit y push → Netlify despliega automáticamente.

---

## Personalizar la animación de partículas

El código está en `src/components/Hero.astro`, dentro de la etiqueta `<script>`.

| Variable     | Qué controla                          | Valor actual |
|--------------|---------------------------------------|--------------|
| `COUNT`      | Cantidad de partículas                | `90`         |
| `MAX_DIST`   | Distancia máxima para conectar puntos | `160`        |
| `MOUSE_DIST` | Radio de influencia del cursor        | `200`        |
| `0.45`       | Velocidad de movimiento               | `0.45`       |
| `0.08`       | Intensidad del efecto parallax        | `0.08`       |

---

## Agregar Google Analytics

En `src/layouts/Layout.astro`, antes del cierre de `</head>`:

```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

Reemplaza `G-XXXXXXXXXX` con tu ID de GA4.

---

## Cambiar el nombre de la agencia

Busca y reemplaza `Nexus Digital` en:
- `src/layouts/Layout.astro` (nav logo + footer logo)
- `src/pages/index.astro` (title)
- `src/data/contacto.json` (footer_copy)
- `astro.config.mjs` (site URL)
