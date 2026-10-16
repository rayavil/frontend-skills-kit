---
name: orquestador-frontend
description: Usar SIEMPRE que Ray empiece un proyecto de frontend nuevo (webapp, dashboard, SaaS, tienda online, landing page, sitio institucional, presentación ejecutiva en navegador, app móvil), pida corregir/mejorar uno que ya se ve "genérico", "feo" o "como de IA", pida auditar/revisar un archivo ya existente para pantalla grande/distancia (TV, proyector, auditorio), o pida verificar/instalar sus dependencias en un equipo nuevo. Esta skill decide por sí sola a cuáles otras skills del kit de Ray recurrir (elegir-ui para plantillas/componentes, frontend-design + taste-skill para dirección de arte, cinematic-gsap-lenis-motion-system + scrollytelling + gsap-skills para animación premium por scroll) según el tipo de proyecto, aplica la marca del cliente si ya existe un perfil guardado, revisa accesibilidad/legibilidad según sea sitio web o presentación, verifica el resultado antes de entregarlo, y hace de intérprete entre lo que Ray describe en términos vagos y las instrucciones técnicas concretas que cada skill necesita. Dispara con frases como "vamos a hacer...", "necesito un sitio/app/tienda/presentación de...", "arma el frontend de...", "esto se ve feo/genérico", "checa que tenga todo instalado", "verifica mis skills de frontend", "audita este archivo para pantalla grande/distancia", "revisa si esto se lee bien en el cañón/TV".
---

# Orquestador de frontend

Esta skill tiene dos trabajos: **decide qué otras skills de Ray usar y en qué orden** según el tipo de proyecto, y **hace de director de arte** — traduce lo que Ray describe en términos vagos ("que se vea premium", "como que la persona va cambiando") a instrucciones técnicas concretas que las skills de ejecución (GSAP, Lenis, etc.) puedan construir. Ray no siempre sabe el nombre técnico de lo que imagina — este paso existe para que no tenga que saberlo.

## Paso 0 — Verifica dependencias (solo la primera vez que se usa en un equipo)

Esta skill depende de otras cinco. Antes de activar el Paso 1 por primera vez en una máquina,
detecta primero **en qué agente estás corriendo** (Claude Code o Antigravity) — la carpeta de
instalación y el flag `--agent` cambian según cuál sea:

| Agente | Carpeta global de skills | Flag para `npx skills add` |
|---|---|---|
| Claude Code | `~/.claude/skills/` | `--agent claude-code` |
| Antigravity | `~/.gemini/config/skills/` | `--agent antigravity` |

Revisa si las dependencias ya están en la carpeta que corresponda a tu agente. Si falta alguna,
instálala tú mismo — no le pidas a Ray que corra esto manualmente, para eso existe este paso.
Sustituye `<CARPETA>` y `<AGENTE>` según la tabla de arriba:

```bash
# elegir-ui (catálogo de UI/plantillas) — viene en el mismo repo que esta skill;
# ya debería estar si clonaste frontend-skills-kit completo, si falta:
cp -r <ruta-del-clon>/frontend-skills-kit/elegir-ui <CARPETA>

# Animación por scroll (motion)
npx skills add https://github.com/greensock/gsap-skills --agent <AGENTE> --global
npx skills add https://github.com/MengTo/Skills --skill cinematic-gsap-lenis-motion-system --agent <AGENTE> --global
npx skills add doodledood/claude-code-plugins --skill scrollytelling --agent <AGENTE> --global

# Dirección de arte / taste
npx skills add https://github.com/Leonxlnx/taste-skill --agent <AGENTE> --global

# Referencia visual — usa --global-agent y --yes, NO corras el comando sin flags:
# sin ellos, skillscat add es interactivo y por defecto instala en los 7 agentes que soporta
# (Cursor, Codex, Gemini, etc.), no solo en el tuyo.
npx skillscat add rshvr/elite-web-design/elite-inspiration --global-agent claude-code --yes
# Si estás en Antigravity, el valor de --global-agent puede no ser literalmente "antigravity"
# (skillscat nombra a los agentes a su manera, ej. puede ser "gemini"). Si no estás seguro de
# cuál es, corre `npx skillscat add --help` primero para ver la lista de agentes soportados en
# vez de adivinar o dejarlo correr interactivo sin supervisión.
```

Si algún comando falla (nombre de paquete cambió, CLI no instalado, el flag `--agent antigravity`
no existe todavía en una versión del CLI, etc.), dile a Ray el error exacto y qué intentaste — no
te quedes callado ni sigas como si hubiera funcionado. Si un paquete de terceros no soporta
instalar directo para Antigravity, cópialo manualmente a la carpeta de Antigravity de la tabla de
arriba (mismo formato `SKILL.md`, solo cambia dónde vive). Una vez instalada una dependencia en
una máquina, no vuelvas a correr su comando ahí.

**Esto se puede pedir solo, sin arrancar un proyecto.** Si Ray dice algo como "checa que tenga
todo instalado en este equipo" o "verifica mis skills de frontend", corre únicamente este Paso 0
y repórtale qué encontraste y qué instalaste — no sigas a los pasos 1-3, no hay proyecto que
clasificar todavía.

## Paso 0.5 — Marca del cliente (antes de la dirección de arte)

Antes de proponer paleta/tipografía/tono (Paso 1.5), revisa si ya existe un perfil de marca para
este cliente en `marcas/<cliente>.yaml` (en esta misma carpeta).

- **Si existe:** úsalo como base de la dirección de arte — no le vuelvas a preguntar a Ray colores
  ni tipografía que ya están documentados. Si el proyecto necesita un matiz distinto (ej. "más
  sobrio" vs "más moderno"), revisa si el archivo ya distingue por tipo de uso (`uso_recomendado`)
  antes de preguntar.
- **Si no existe** y el proyecto es para un cliente/marca recurrente (no un ejercicio de muestra):
  pregúntale a Ray los datos básicos (colores institucionales, tipografía, tono/personalidad de
  marca) y, al terminar el proyecto, ofrece guardarlos en un `marcas/<cliente>.yaml` nuevo (mismo
  formato que `marcas/itsf.yaml`) para no tener que preguntarlos otra vez la próxima vez.
- **Si es un ejercicio de muestra** (ej. "una cafetería de prueba" sin cliente real detrás), no
  apliques esto — ahí la dirección de arte se decide desde cero en el Paso 1.5.

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

**C. Sitio premium / landing / presentación ejecutiva → usa `frontend-design` + `taste-skill` (dirección de arte) + `gsap-skills` + `cinematic-gsap-lenis-motion-system` + `scrollytelling` (ejecución técnica)**
Esta categoría SIEMPRE lleva dos capas, nunca solo la técnica — ver Paso 1.5 antes de construir aquí.

**D. App móvil / híbrida → usa `elegir-ui`**
Filtra por categoría `app_movil`.

## Paso 1.5 — Dirección de arte (solo categoría C, o cuando Ray pida corregir algo que "se ve feo/genérico")

Antes de tocar código:

1. **Entiende el tono que busca Ray**, aunque lo describa vago. Si no dio referencias, pregúntale 1-2 cosas cortas: ¿tiene un sitio de referencia en mente (como palominoprod.com o jeffmilanes.com que ya usamos antes)? ¿es más minimalista/editorial o más oscuro/cinemático?
2. **Consulta `frontend-design` y las skills de `taste-skill`** (`redesign-skill` si ya existe un archivo que corregir, `soft-skill` para tono premium/lujo, `minimalist-skill` para editorial) para fijar: paleta, tipografía, densidad visual, nivel de "experimentación" del layout. Esto define el **look en reposo** — cómo se ve el sitio sin animación. Si el Paso 0.5 encontró un perfil de marca, esta paleta/tipografía parte de ahí, no se inventa.
3. **Traduce ese look a una lista de efectos concretos** usando `tecnicas-animacion.md` (en esta misma carpeta). Si Ray describe un efecto sin saber su nombre (ej. "que la persona vaya cambiando con el scroll"), identifica cuál técnica del catálogo es y dilo en voz alta: *"eso es scroll-driven frame scrubbing — se puede, pero necesito que me des la secuencia de imágenes/fotos de la persona en cada pose."* Si la técnica requiere material que Ray no ha dado (fotos, video, secuencia de frames), pídelo ANTES de construir, no a medias.
4. **Checklist de contenido real** (para TODAS las categorías, no solo cuando la animación lo exige): antes de escribir código, dile a Ray qué vas a necesitar y pregunta qué de eso ya tiene — copy/textos reales, logos, fotos propias, datos/cifras, info de contacto. Si algo no lo tiene todavía, acláraselo (ej. "voy a usar texto de relleno en la sección de testimonios, avísame cuando tengas los reales") en vez de entregar un demo con contenido inventado sin decir cuál es cuál.
5. **Decide si esto es sitio web o presentación** y aplica las reglas que correspondan de `accesibilidad.md` (en esta misma carpeta) — las reglas de accesibilidad web (contraste, teclado, `prefers-reduced-motion`, tamaño de touch targets) NO son las mismas que las directrices de diseño de presentación (tamaño de texto legible a distancia, tratamiento de imágenes/gráficos, densidad de información tipo slide). Una presentación para proyectar en un auditorio sigue reglas de presentación, no de sitio web, aunque técnicamente sea una página HTML. Si es presentación, pregunta o infiere además el escenario de distancia (monitor propio / TV-sala de juntas / proyector-auditorio) — `accesibilidad.md` trae una tabla de tamaños mínimos concretos por escenario, úsala para fijar los `font-size` y tamaños de ícono/botón desde el principio, no los ajustes después.
6. **Entrega las instrucciones técnicas ya traducidas** a las skills de ejecución (GSAP/Lenis/ScrollTrigger) — ellas no deciden el diseño, solo ejecutan lo que esta capa ya definió.

## Paso 3 — Resume la decisión antes de construir

Antes de escribir código, dile a Ray en una línea qué ruta tomaste y por qué, por ejemplo:
> "Esto es más presentación que panel funcional, así que voy con tono editorial/minimalista (taste-skill) + scroll-driven frame scrubbing para la portada, y GSAP+Lenis para las transiciones entre secciones."

Esto le permite corregirte rápido si clasificaste mal el proyecto, o si la dirección de arte no es la que imaginaba, antes de que inviertas tiempo construyendo.

## Paso 4 — Verifica antes de decir que ya está

No reportes el entregable como terminado sin antes revisar esto:

1. **Revisión estática del código**: busca en el HTML/JS generado los valores de `pin`, `end` y
   `pinSpacing` de cada `ScrollTrigger` y confirma que el `end` no exceda la altura real del
   contenido anclado (esto fue exactamente el bug que salió en la versión anterior del sitio ITSF).
   Confirma también que existe `<meta name="viewport">` y que las animaciones respetan
   `prefers-reduced-motion` (ver `accesibilidad.md`).
2. **Auditoría de tamaños, si es presentación (categoría C en modo presentación)**: revisa en el
   CSS generado los `font-size` de títulos, cuerpo y cifras destacadas, y los tamaños de íconos y
   botones, contra la tabla de mínimos por escenario de `accesibilidad.md` (monitor propio / TV-sala
   de juntas / proyector-auditorio) para el escenario que se fijó en el Paso 1.5. Confirma también
   que esos tamaños están definidos con `clamp()`/unidades relativas (`vw`), no `px` fijo — un `px`
   fijo no garantiza el mismo tamaño físico en una pantalla/proyector distinto al que probaste. Si
   algún valor queda por debajo del mínimo de ese escenario, o está en `px` fijo donde debería ser
   fluido, corrígelo antes de entregar — no lo dejes para que Ray lo note hasta que ya esté frente
   al grupo con el cañón.
3. **Revisión visual**: si tienes una herramienta de navegador disponible en este entorno (Claude
   in Chrome o el navegador integrado), abre el archivo ahí, haz scroll completo de principio a
   fin y revisa la consola en busca de errores. Si no tienes navegador disponible, dile a Ray
   explícitamente qué probar él mismo antes de darlo por bueno (ej. "ábrelo y haz scroll completo,
   checa que la sección 2 no brinque al final del pin").
4. Si encuentras un problema, corrígelo antes de entregar — no lo reportes como "ya quedó" con un
   pendiente conocido sin decírselo a Ray explícitamente.

**Esto también se puede pedir solo, sobre un archivo que ya existe** (no solo recién construido en
esta misma conversación). Si Ray dice algo como "audita este archivo para pantalla grande/distancia",
"revísalo para que se lea en el cañón/TV" o "checa si esto cumple los tamaños de presentación",
pregúntale (si no lo dijo ya) el escenario de distancia, corre solo los puntos 2 y 3 de este Paso 4
sobre ese archivo, y repórtale qué encontraste — no hace falta reclasificar el proyecto desde el
Paso 1 ni repetir la dirección de arte del Paso 1.5.

## Paso 5 — Integración a tu stack (opcional, solo si Ray lo pide o aprueba el demo)

El HTML autocontenido es para validar rápido el diseño y la animación. Una vez que Ray lo aprueba
y el proyecto va a producción real (no un ejercicio de muestra), ofrécele convertirlo a
componente(s) Vue 3 o React para integrarlo a su stack Laravel + Inertia.js — no asumas que el
HTML suelto es el entregable final de un proyecto real; pregunta si quiere la versión componentizada
ahora o más adelante.

## Mantenimiento

Si Ray dice que un proyecto no encajó bien en ninguna categoría, o que la combinación de skills no fue la correcta, ajusta la tabla de clasificación del Paso 1 con la nueva señal que debiste haber detectado.
