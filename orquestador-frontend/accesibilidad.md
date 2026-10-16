# Accesibilidad y legibilidad — reglas separadas por tipo de entregable

Un sitio web y una presentación en navegador se consumen distinto, así que tienen reglas
distintas. No apliques las de una al otro — revisa primero cuál de los dos estás construyendo
(Paso 1.5 del orquestador).

## A. Reglas de accesibilidad web (categorías A, B, D, y la C cuando es un sitio real)

Se consume en pantalla propia, de cerca, con mouse/teclado/táctil, posiblemente por alguien con
una discapacidad visual o motriz. Antes de entregar:

- **Contraste de texto**: mínimo AA (4.5:1 para texto normal, 3:1 para texto grande/18px+bold).
  Si la paleta de marca no cumple en algún fondo, ajusta el tono en vez de bajar la opacidad del
  texto para "que se vea más suave" — eso rompe el contraste.
- **Navegación por teclado**: todo elemento interactivo (botones, links, formularios) debe ser
  alcanzable con Tab y tener un estado `:focus-visible` claro. Si hay cursor personalizado
  reactivo o magnetic hover, confirma que no reemplacen o escondan el foco de teclado.
- **`prefers-reduced-motion`**: toda animación de scroll (ScrollTrigger, parallax, frame
  scrubbing) debe tener una alternativa reducida o estática cuando el sistema operativo pide
  menos movimiento. No es opcional en sitios reales — sí puede omitirse en un demo de muestra si
  Ray lo sabe y lo acepta.
- **Tamaño de touch targets**: mínimo 44×44px en botones/links para uso en móvil.
- **Texto alternativo**: toda imagen con contenido informativo (no decorativa) necesita `alt`
  descriptivo.

## B. Directrices de diseño de presentación (categoría C cuando es para proyectar/presentar, no un sitio)

Se consume de lejos (proyector, pantalla en sala de juntas, auditorio), normalmente sin que la
audiencia controle el ritmo — reglas de slide, no de página web:

- **Tamaño de texto, según distancia real de visualización** — "letra grande" no es un número
  fijo, depende de qué tan lejos está quien la lee. Antes de construir, pregúntale a Ray (o
  infiérelo del contexto: "para llevar a bachilleratos" = auditorio/salón de clases, no monitor)
  en cuál de estos tres escenarios se va a ver, y usa esos mínimos como punto de partida. Están
  calculados a partir de dos fuentes reales de diseño de señalización/presentaciones, no
  inventados — ver [fuentes](#fuentes) al final:

  | Escenario | Distancia típica | Título | Cuerpo de texto | Cifras/números destacados |
  |---|---|---|---|---|
  | Monitor/laptop propio | <1 m | ≥32px | ≥18px | ≥40px |
  | TV o pantalla en sala de juntas | 2-5 m | ≥64px | ≥32px | ≥90px |
  | Proyector en auditorio/aula | 5-15+ m | ≥110px | ≥48px | ≥160px |

  Si no sabes la distancia exacta, usa el escenario más exigente que sea razonable para el
  contexto (una gira de bachilleratos casi siempre es auditorio/aula, no monitor). Nunca asumas
  el escenario de monitor por default para algo descrito como "presentación" — ese es justo el
  error que hace que algo se vea bien en tu laptop y sea illegible en el cañón del plantel.

  **Corrección técnica importante — no uses `px` fijos para esto.** Un tamaño de texto en `px`
  no garantiza el mismo tamaño físico en pantalla: la legibilidad real depende del tamaño físico
  de la pantalla/proyección y la distancia, no de su resolución en píxeles — un proyector 720p y
  uno 4K del mismo tamaño de imagen necesitan el mismo tamaño físico de letra, pero en píxeles
  CSS esos números son distintos. Para presentación (sección B), define la tipografía con
  unidades fluidas atadas al viewport (`vw` o `clamp()`), nunca `px` fijo:
  ```css
  h1 { font-size: clamp(64px, 6vw, 140px); }   /* título: nunca baja de 64px ni sube de 140px */
  p  { font-size: clamp(28px, 2.5vw, 56px); }  /* cuerpo */
  ```
  Así, si conoces las dimensiones de la pantalla/proyector real del evento, puedes verificar con
  la **regla del diagonal**: un texto es legible con comodidad si ningún espectador está a más de
  ~2.5× la diagonal de la pantalla; hasta ~5-6× la diagonal sigue siendo detectable para títulos
  grandes/cifras, aunque no para leer texto corrido. Si Ray te da el tamaño de pantalla/proyección
  y la distancia de la última fila, puedes calcular el mínimo exacto en vez de usar la tabla
  genérica de arriba.

- **Tamaño de componentes e iconos**: igual que el texto, escala con la distancia — un ícono o
  botón que se ve bien de cerca puede desaparecer visualmente de lejos. Para auditorio/TV, ningún
  elemento interactivo o ícono informativo debería ser menor a ~48px efectivos; en proyector,
  súbelo a ~80px. Igual que el texto, defínelos con `clamp()`/unidades relativas, no `px` fijo.
  Evita filas de iconos pequeños o badges con texto microscópico — si hay varios elementos,
  prefiere menos elementos más grandes a muchos pequeños.

- **Densidad de información por sección/"slide"**: una idea central por sección, no un párrafo
  largo. Si el contenido real (ej. documentación institucional) trae mucho texto, resume y deja el
  detalle para un documento de apoyo aparte — la presentación no es el lugar para texto corrido.

- **Tratamiento de imágenes y gráficos**: fotos y gráficos a tamaño generoso, nunca como miniatura
  decorativa — en una presentación la imagen suele ser el punto, no un acento. Las cifras clave
  (count-up numérico) se muestran grandes y solas, no metidas en una tabla densa. Para proyector/TV,
  evita gráficos con muchas líneas finas, leyendas chicas o etiquetas de ejes densas — se pierden
  por completo a distancia; prefiere pocas series, etiquetas grandes y valores destacados en vez de
  una gráfica "completa" como la verías en un dashboard. Para cualquier gráfico/chart, sigue el
  skill `dataviz` en vez de inventar colores/ejes a mano, pero ajusta tamaños de fuente/trazo al
  escenario de distancia de la tabla de arriba.
- **Contraste pensado para proyector**: los proyectores lavan el contraste y saturan menos que una
  pantalla — evita combinaciones de bajo contraste que en tu monitor se ven bien pero que en un
  proyector de salón con luz ambiente se vuelven ilegibles (fondo oscuro con texto gris medio es
  el error más común).
- **Ritmo controlado por quien presenta, no por el público**: si hay scroll, confirma con Ray si
  alguien va a operar el scroll en vivo frente a una audiencia (necesita ser predecible y no
  "saltar" secciones) o si se va a dejar para que la gente navegue sola (ahí sí aplican más reglas
  de la sección A).

### Uso creativo de pantalla grande (no es solo "ponerlo más grande")

Diseñar para proyector/auditorio no es la misma pieza que para monitor escalada hacia arriba —
una pantalla grande permite recursos que una pantalla propia no:

- **Usa sets de imágenes, no una foto sola por sección.** Un montage de 3-4 fotos relacionadas
  (campus, estudiantes, egresados) suele pegar más fuerte en pantalla grande que una sola imagen
  grande — siempre que planees el espacio para el montage completo desde el inicio, no lo
  encimes después.
- **Anima las cifras/gráficas en etapas, no las dejes estáticas.** Un dato que aparece de golpe
  se pierde; un count-up o una barra que crece mientras se habla de ella mantiene la atención del
  público en la pantalla — esto ya es lo que hace la técnica "count-up numérico" del catálogo de
  animaciones, pero aplícalo también a cualquier gráfica, no solo a números sueltos.
- **Diseña en 16:9 y dejá un margen de seguridad en los bordes.** Muchos proyectores recortan
  ligeramente la imagen (overscan) — no pongas texto o logos pegados al borde de la pantalla.
- **Evita fondos blancos sólidos en salas con poca luz.** Un fondo blanco de pantalla completa
  encandila y compite con la iluminación del lugar; un fondo oscuro con acentos de color (como la
  paleta institucional) se ve mejor en auditorio real que en tu monitor.
- **Diseña una identidad propia para esa presentación, no un template genérico.** Esto es lo que
  ya hace la capa de dirección de arte (Paso 1.5) — en pantalla grande un diseño genérico se nota
  todavía más que en un sitio web chico.
- **Prueba práctica antes de dar por bueno el tamaño**: si no tienes acceso a probarlo en la
  pantalla real, una aproximación rápida es revisarlo a distancia — aléjate físicamente de tu
  monitor unos pasos, o revísalo en el teléfono sostenido con el brazo extendido — si algo ya se
  ve chico así, en un auditorio real va a desaparecer.

## Cómo decidir cuál aplica

Pregúntale a Ray (o infiere del contexto) si el entregable se va a **ver en una pantalla propia,
navegando libremente** (aplica A) o **presentar frente a una audiencia, en una pantalla
compartida** (aplica B). Un sitio institucional como itsf.mx es A. Una presentación para la gira de
promoción a bachilleratos es B. Si mezcla ambos usos (ej. se presenta en vivo pero luego se sube
al sitio para que la gente la revise sola), aplica ambas reglas y prioriza B para el tamaño de
texto — lo que es legible de lejos también es legible de cerca, no al revés.

Si aplica B, no te quedes en "es presentación" — pregunta o infiere también **en qué escenario
específico** de la tabla de tamaños de arriba (monitor propio / TV-sala de juntas / proyector-
auditorio), porque cambia los números concretos que vas a usar en el CSS.

## Fuentes

- [MVIX — Recommended Font and Screen Sizes for Signage Displays](https://www.mvix.com/knowledgebase/recommended-font-and-screen-sizes) — tabla de tamaño de fuente por distancia de visualización en señalización digital, base de los mínimos de la tabla de arriba.
- [Screen Size For Presentations (Universität Freiburg)](https://ad-wiki.informatik.uni-freiburg.de/research/HowTos/ScreenSizeForPresentations) — la "regla del diagonal" (2.5× la diagonal de pantalla para lectura cómoda, hasta ~5-6× para detección).
- [Beautiful.ai / Slidegenius — font size & large-screen display guidance](https://www.slidegenius.com/blog/displaying-slide-presentations-using-large-screens) — mínimos de 30pt/20pt, 16:9, resolución mínima 1920×1080, alto contraste.
- [Yes& Agency — 11 Tips for Creating a Captivating Big Screen Presentation](https://insights.yesandagency.com/11-tips-creating-captivating-big-screen-presentation) — sets de imágenes, animar gráficas en etapas, margen de seguridad por overscan, evitar fondos blancos sólidos.
- [CommonsWare — Presenters, Please Use Bigger Fonts](https://commonsware.com/blog/2019/07/24/presenters-please-use-bigger-fonts.html) — prueba práctica de "verlo en el teléfono a distancia de brazo" como aproximación rápida sin acceso a la pantalla real.
