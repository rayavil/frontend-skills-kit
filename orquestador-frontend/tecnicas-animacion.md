# Catálogo de técnicas de animación por scroll

Menú de efectos con nombre, para señalar en vez de describir. Todas son reproducibles con
GSAP + ScrollTrigger + Lenis (ya instalados). Cuando Ray pida "algo como [nombre]", el agente
busca una referencia visual actual del efecto antes de construir, para confirmar que es lo que
Ray imagina.

| Técnica | Qué se ve | Ejemplo típico | Complejidad |
|---|---|---|---|
| **Scroll-driven frame scrubbing** | Una secuencia de imágenes (cientos de PNG) se reproduce cuadro por cuadro atada al scroll — el objeto "gira" o "cambia" conforme avanzas. Esto es lo que describiste del personaje que se va transformando. | Apple (giro del iPhone), Nike SNKRS | Alta — requiere generar/tener la secuencia de imágenes |
| **Pinning (anclado)** | Una sección se queda fija en pantalla mientras el contenido de adentro cambia o el fondo se mueve detrás. | Ya está en tu ejemplo de ITSF (secciones 2-4) | Media |
| **Horizontal scroll** | El scroll vertical normal se convierte en movimiento horizontal dentro de una sección — como una galería que se recorre de lado. | Portafolios de estudios creativos, Awwwards | Media |
| **Text reveal / stagger** | El texto aparece letra por letra o palabra por palabra conforme entra a pantalla. | Titulares de landing pages premium | Baja |
| **Parallax multicapa** | Varias capas (fondo, texto, objetos) se mueven a velocidades distintas al hacer scroll, dando sensación de profundidad. | Casi cualquier sitio Awwwards | Baja-media |
| **Mask reveal / clip-path** | Una imagen o texto se "revela" como si una cortina se abriera, usando máscaras CSS. | Sitios editoriales, revistas digitales | Media |
| **Magnetic hover** | Botones o elementos que "persiguen" el cursor cuando te acercas, como si tuvieran imán. | Ya está en tu ejemplo de ITSF (botones) | Baja |
| **Sticky side-by-side** | Una columna se queda fija (ej. una imagen o gráfica) mientras la columna de al lado hace scroll normal con el texto. | Artículos explicativos tipo Pudding.cool, NYT interactivos | Media |
| **Smooth scroll con inercia** | El scroll deja de sentirse "brusco" del navegador y gana un efecto de desaceleración suave. | Es la base de Lenis, ya está en tu ejemplo | Baja |
| **Cursor personalizado reactivo** | El cursor cambia de forma/tamaño según sobre qué elemento pasa. | Ya está en tu ejemplo de ITSF | Baja |
| **3D tilt / perspective cards** | Tarjetas que se inclinan en 3D según la posición del mouse, dando sensación de profundidad física. | Portafolios creativos, Stripe | Media |
| **Video scroll-sync** | Un video (no imágenes sueltas) se reproduce cuadro por cuadro atado al scroll, en vez de reproducirse solo. | Páginas de producto de Apple más recientes | Alta — requiere el video pre-grabado |
| **Marquee / scroll infinito horizontal** | Texto o logos que se desplazan en bucle continuo, a veces acelerando con la velocidad del scroll del usuario. | Secciones de "clientes" o "marcas" | Baja |
| **Count-up numérico** | Números que suben animados desde 0 hasta su valor final cuando entran a pantalla. | Ya está en tu ejemplo de ITSF (métricas TecNM) | Baja |

## Nota sobre complejidad

- **Baja/Media:** el agente las construye directo, sin material adicional de tu parte.
- **Alta (frame scrubbing, video scroll-sync):** necesitas la secuencia de imágenes o el video de antemano — esto no lo genera la IA, hay que producirlo o conseguirlo (banco de imágenes, fotografía propia, o un render 3D).

## Cómo pedirlo

En vez de describir el efecto, dilo así: *"quiero la sección de portada con scroll-driven frame
scrubbing"* o *"esta parte con sticky side-by-side"* — el agente ya sabe qué construir y con qué
herramienta de las que tenemos instaladas.
