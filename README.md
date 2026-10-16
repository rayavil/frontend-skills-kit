# frontend-skills-kit

Kit de skills de agentes de IA (Claude Code, Antigravity) para arrancar proyectos de frontend —
elección de sistemas de UI/plantillas open source y animación premium por scroll — alineado al
stack de Ray / Devizac (Laravel + Inertia.js + Vue 3 / React + Tailwind).

## Qué hay aquí

| Skill | Qué hace |
|---|---|
| `orquestador-frontend/` | Router + director de arte: al describir un proyecto nuevo, decide sola cuáles otras skills usar (catálogo de UI, dirección de arte, animación de scroll), aplica la marca del cliente si ya existe (`marcas/`), revisa accesibilidad/legibilidad según sea sitio o presentación (`accesibilidad.md`), verifica el resultado antes de entregarlo, y traduce lo que describes en términos vagos a instrucciones técnicas concretas. **Instálala — ella instala las demás sola la primera vez que la usas** (ver su Paso 0). |
| `elegir-ui/` | Catálogo curado (`catalogo.yaml`, 30+ entradas) de librerías de componentes y plantillas open source gratuitas, organizadas por categoría (dashboard, admin panel, landing, e-commerce, app móvil), con licencia, estado de mantenimiento y "top pick" por categoría. |

## Skills de terceros que completan el kit

No se copian aquí (son de otros autores). `orquestador-frontend` las instala sola la primera vez
que se activa en un equipo nuevo (su Paso 0 trae los comandos exactos) — no necesitas correrlos
tú a mano:

- `gsap-skills`, `cinematic-gsap-lenis-motion-system`, `scrollytelling` — animación por scroll
- `taste-skill` (familia: taste/redesign/soft/minimalist) — dirección de arte
- `elite-inspiration` — índice de referencia visual

## Instalación en un equipo nuevo

### Claude Code

```bash
git clone https://github.com/rayavil/frontend-skills-kit.git
mkdir -p ~/.claude/skills
cp -r frontend-skills-kit/elegir-ui ~/.claude/skills/
cp -r frontend-skills-kit/orquestador-frontend ~/.claude/skills/
```

Con eso basta. La primera vez que le pidas algo de frontend a Claude Code, `orquestador-frontend`
revisa qué falta y lo instala — ver su Paso 0 si quieres correrlo tú manualmente de una vez en
lugar de esperar a la primera activación.

### Antigravity

Mismo formato de `SKILL.md`, solo cambia la carpeta destino:

```bash
# Global
mkdir -p ~/.gemini/config/skills
cp -r frontend-skills-kit/elegir-ui ~/.gemini/config/skills/
cp -r frontend-skills-kit/orquestador-frontend ~/.gemini/config/skills/

# O por proyecto
mkdir -p .agents/skills
cp -r frontend-skills-kit/elegir-ui .agents/skills/
cp -r frontend-skills-kit/orquestador-frontend .agents/skills/
```

## Cómo actualizar cuando hay cambios nuevos en el repo

Con `cp`, cada `git pull` requiere volver a copiar las carpetas a mano. Hay dos formas de
traer los cambios:

**Opción rápida (si instalaste con `cp`):**

```bash
cd frontend-skills-kit
git pull
cp -r elegir-ui/* ~/.claude/skills/elegir-ui/
cp -r orquestador-frontend/* ~/.claude/skills/orquestador-frontend/
```

(cambia la carpeta destino a `~/.gemini/config/skills/` si es Antigravity)

**Opción recomendada (una sola vez, nunca vuelves a copiar):** usa symlinks en vez de `cp`
al instalar. Así un `git pull` en el repo clonado actualiza las skills sin ningún paso extra:

```bash
# Si ya instalaste con cp, primero quita las copias:
rm -rf ~/.claude/skills/elegir-ui ~/.claude/skills/orquestador-frontend

# Symlink a la ruta real donde clonaste el repo:
ln -s /ruta/a/frontend-skills-kit/elegir-ui ~/.claude/skills/elegir-ui
ln -s /ruta/a/frontend-skills-kit/orquestador-frontend ~/.claude/skills/orquestador-frontend
```

A partir de ahí, actualizar en cualquier equipo es solo:

```bash
cd frontend-skills-kit && git pull
```

## Mantenimiento

Cuando pruebes una plantilla nueva o una ya catalogada deje de servir, dile al agente
"agrega esto al catálogo" o "marca X como que no funcionó" — las instrucciones de
`elegir-ui/SKILL.md` ya le dicen cómo editar `catalogo.yaml`. Haz commit y push del cambio
para que se refleje en todos tus equipos.
