# frontend-skills-kit

Kit de skills de agentes de IA (Claude Code, Antigravity) para arrancar proyectos de frontend —
elección de sistemas de UI/plantillas open source y animación premium por scroll — alineado al
stack de Ray / Devizac (Laravel + Inertia.js + Vue 3 / React + Tailwind).

## Qué hay aquí

| Skill | Qué hace |
|---|---|
| `orquestador-frontend/` | Router: al describir un proyecto nuevo, decide sola cuáles otras skills usar (catálogo de UI, o animación de scroll, o ambas). **Instálala para que las demás se activen solas.** |
| `elegir-ui/` | Catálogo curado (`catalogo.yaml`, 30+ entradas) de librerías de componentes y plantillas open source gratuitas, organizadas por categoría (dashboard, admin panel, landing, e-commerce, app móvil), con licencia, estado de mantenimiento y "top pick" por categoría. |

## Skills de terceros que completan el kit

Estas no se copian aquí (son de otros autores, se instalan directo desde su fuente). El
`orquestador-frontend` las referencia por nombre, así que instálalas también si quieres el flujo
completo de animación premium por scroll:

```bash
npx skills add https://github.com/greensock/gsap-skills --agent claude-code --global
npx skills add https://github.com/MengTo/Skills --skill cinematic-gsap-lenis-motion-system --agent claude-code --global
npx skills add doodledood/claude-code-plugins --skill scrollytelling --agent claude-code --global
```

## Instalación en un equipo nuevo

### Claude Code

```bash
git clone https://github.com/rayavil/frontend-skills-kit.git
mkdir -p ~/.claude/skills
cp -r frontend-skills-kit/elegir-ui ~/.claude/skills/
cp -r frontend-skills-kit/orquestador-frontend ~/.claude/skills/
```

(Agrega las 3 skills de terceros de arriba también, con `--global`.)

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

## Mantenimiento

Cuando pruebes una plantilla nueva o una ya catalogada deje de servir, dile al agente
"agrega esto al catálogo" o "marca X como que no funcionó" — las instrucciones de
`elegir-ui/SKILL.md` ya le dicen cómo editar `catalogo.yaml`. Haz commit y push del cambio
para que se refleje en todos tus equipos.
