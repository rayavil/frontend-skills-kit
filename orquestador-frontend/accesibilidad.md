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

- **Tamaño de texto**: títulos nunca por debajo de ~48px efectivos en pantalla completa, cuerpo de
  texto nunca por debajo de ~24px — lo que se ve bien en un monitor de escritorio de cerca puede
  ser ilegible a 5 metros en un auditorio.
- **Densidad de información por sección/"slide"**: una idea central por sección, no un párrafo
  largo. Si el contenido real (ej. documentación institucional) trae mucho texto, resume y deja el
  detalle para un documento de apoyo aparte — la presentación no es el lugar para texto corrido.
- **Tratamiento de imágenes y gráficos**: fotos y gráficos a tamaño generoso, nunca como miniatura
  decorativa — en una presentación la imagen suele ser el punto, no un acento. Las cifras clave
  (count-up numérico) se muestran grandes y solas, no metidas en una tabla densa.
  Para cualquier gráfico/chart, sigue el skill `dataviz` en vez de inventar colores/ejes a mano.
- **Contraste pensado para proyector**: los proyectores lavan el contraste y saturan menos que una
  pantalla — evita combinaciones de bajo contraste que en tu monitor se ven bien pero que en un
  proyector de salón con luz ambiente se vuelven ilegibles (fondo oscuro con texto gris medio es
  el error más común).
- **Ritmo controlado por quien presenta, no por el público**: si hay scroll, confirma con Ray si
  alguien va a operar el scroll en vivo frente a una audiencia (necesita ser predecible y no
  "saltar" secciones) o si se va a dejar para que la gente navegue sola (ahí sí aplican más reglas
  de la sección A).

## Cómo decidir cuál aplica

Pregúntale a Ray (o infiere del contexto) si el entregable se va a **ver en una pantalla propia,
navegando libremente** (aplica A) o **presentar frente a una audiencia, en una pantalla
compartida** (aplica B). Un sitio institucional como itsf.mx es A. Una presentación para la gira de
promoción a bachilleratos es B. Si mezcla ambos usos (ej. se presenta en vivo pero luego se sube
al sitio para que la gente la revise sola), aplica ambas reglas y prioriza B para el tamaño de
texto — lo que es legible de lejos también es legible de cerca, no al revés.
