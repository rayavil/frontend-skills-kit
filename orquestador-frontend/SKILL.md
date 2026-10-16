---
name: orquestador-frontend
description: Usar SIEMPRE que Ray empiece un proyecto de frontend nuevo (webapp, dashboard, SaaS, tienda online, landing page, sitio institucional, presentación ejecutiva en navegador, app móvil). Esta skill decide por sí sola a cuáles otras skills del kit de Ray recurrir (elegir-ui para plantillas/componentes, cinematic-gsap-lenis-motion-system + scrollytelling + gsap-skills para animación premium por scroll) según el tipo de proyecto, para que Ray no tenga que invocarlas una por una. Dispara con frases como "vamos a hacer...", "necesito un sitio/app/tienda/presentación de...", "arma el frontend de...".
---

# Orquestador de frontend

Esta skill es un **router**: no decide el diseño en sí, decide **qué otras skills de Ray usar y en qué orden** según el tipo de proyecto que describe.

## Paso 1 — Clasifica el proyecto

Lee la descripción de Ray y ubícalo en una (o más) de estas categorías:

| Categoría | Señales típicas | Ejemplos |
|---|---|---|
| **A. App funcional / SaaS / dashboard / admin** | "panel", "dashboard", "sistema para gestionar...", "CRM", "intranet", datos, tablas, formularios, login | Sistema de citas, CRM interno, panel de control |
| **B. Tienda online** | "tienda", "catálogo de productos", "carrito", "checkout" | E-commerce, catálogo de productos |
| **C. Sitio premium / landing / presentación ejecutiva** | "landing", "presentación", "sitio institucional de alto nivel", "que se vea premium/Awwwards", scroll, storytelling, portafolio | Landing de producto, presentación para inversionistas, sitio de marca |
| **D. App móvil / híbrida** | "app móvil", "PWA", mención de iOS/Android | App para clientes, PWA |

Si el proyecto mezcla categorías (ej. una tienda online que además quiere una landing premium de entrada), trátalo como las dos: la landing usa el plan C, el catálogo/checkout usa el plan B.

Si no queda claro, pregúntale a Ray en una sola pregunta corta antes de arrancar (ej. "¿esto es más panel funcional o más vitrina/presentación?").

## Paso 2 — Activa las skills correspondientes

**A. App funcional / SaaS / dashboard / admin → usa `elegir-ui`**
Lee `catalogo.yaml` de esa skill, filtra por `dashboard` o `admin_panel` según corresponda, prioriza `top_pick`. No uses animación de scroll pesada aquí — estas interfaces se usan a diario, priorizar velocidad y funcionalidad sobre espectáculo visual.

**B. Tienda online → usa `elegir-ui`**
Filtra por categoría `ecommerce`. Si Ray además quiere una landing de entrada vistosa antes del catálogo, combina con el plan C solo para esa portada.

**C. Sitio premium / landing / presentación ejecutiva → usa `gsap-skills` + `cinematic-gsap-lenis-motion-system` + `scrollytelling`**
Para la base de animación (Lenis + ScrollTrigger + pinning). Para la paleta, tipografía y componentes base (botones, cards, nav), consulta igualmente `catalogo.yaml` de `elegir-ui` en la categoría `landing_page` — no inventes componentes desde cero si ya hay uno probado en el catálogo que encaje con el estilo.

**D. App móvil / híbrida → usa `elegir-ui`**
Filtra por categoría `app_movil`.

## Paso 3 — Resume la decisión antes de construir

Antes de escribir código, dile a Ray en una línea qué ruta tomaste y por qué, por ejemplo:
> "Esto es más presentación que panel funcional, así que voy con el sistema de motion (GSAP+Lenis) para las transiciones y tomo la paleta/tipografía institucional que ya tenemos."

Esto le permite corregirte rápido si clasificaste mal el proyecto antes de que inviertas tiempo construyendo.

## Mantenimiento

Si Ray dice que un proyecto no encajó bien en ninguna categoría, o que la combinación de skills no fue la correcta, ajusta la tabla de clasificación del Paso 1 con la nueva señal que debiste haber detectado.
