---
name: elegir-ui
description: Usar cuando Ray esté empezando un proyecto de desarrollo nuevo (web, SaaS, dashboard, landing, tienda, app móvil) y necesite elegir un sistema de UI, librería de componentes o plantilla open source gratuita acorde al tipo de proyecto y a su stack (Laravel + Inertia.js + Vue 3 / React + Tailwind). Dispara con frases como "vamos a hacer un/a app de...", "qué uso para el frontend de...", "necesito un dashboard/admin/landing para...".
---

# Elegir UI y plantillas

Esta skill recomienda sistemas de UI y plantillas open source **100% gratuitas o con capa gratuita suficiente**, usando el catálogo curado en `catalogo.yaml`, filtrado por el stack de Ray (Laravel + Inertia.js, Vue 3 o React, Tailwind).

## Flujo

1. **Identifica el tipo de proyecto** a partir de lo que describe Ray (ej. "app deportiva", "tienda online", "CRM interno").
2. **Lee `catalogo.yaml`** y usa la sección `mapeo_proyecto_componentes` para traducir el tipo de proyecto a una o más categorías del catálogo (`starter_kit`, `dashboard`, `admin_panel`, `landing_page`, `ecommerce`, `app_movil`). Si el tipo de proyecto no está en el mapeo, infiere la categoría más cercana y dilo explícitamente.
3. **Filtra por framework** según lo que diga Ray o, si no lo dice, pregunta Vue 3 o React (su stack soporta ambos vía Inertia).
4. **Prioriza:**
   - Primero las entradas con `top_pick: true`.
   - Nunca recomiendes una entrada con `estado: legacy` como primera opción — menciónala solo si Ray pregunta explícitamente o no hay alternativa viva en esa categoría, y advierte que está abandonada.
   - Si una entrada tiene `estado: mantenimiento_moderado`, acláralo (no la descartes, pero avisa).
5. **Presenta 2-3 opciones máximo** por categoría relevante, no la lista completa. Para cada una da: nombre, por qué encaja con el caso, tipo (plantilla vs librería), link al repo y a la demo si existe, y el comando de instalación.
6. **Si es "plantilla"**, aclara que es una base ya armada (ahorra tiempo, pero hay que acoplarla a Laravel/Inertia). Si es "librería", aclara que son piezas para ensamblar tú (más control, más trabajo de armado).
7. **Verifica antes de prometer**: si ha pasado tiempo desde la última actualización del catálogo o la entrada no tiene `top_pick`, sugiere confirmar con una búsqueda rápida que el repo siga vivo antes de que Ray invierta tiempo instalándolo.

## Actualizar el catálogo

Cuando Ray mencione una plantilla o librería nueva que le gustó o usó, o cuando una entrada existente demuestre estar desactualizada, agrega/edita la entrada en `catalogo.yaml` siguiendo el mismo formato de campos. Si Ray da una opinión sobre una plantilla ya probada ("esa no me gustó", "esta me resolvió rápido"), añade esa nota al campo `nota` de la entrada correspondiente.

## Ejemplo de uso

Ray: "Vamos a hacer una app deportiva para un cliente"
→ Categorías: `dashboard` + `admin_panel` (necesita: cards de stats, gráficas, calendario de partidos, tablas de posiciones, perfiles)
→ Top picks a proponer: Vuestic Admin (si es Vue) o shadcn-admin-kit/Studio Admin (si es React) para la base del panel, + Tremor o PrimeVue DataTable para las gráficas/tablas específicas.
