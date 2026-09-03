# agents-hermes

- `~/.hermes/SOUL.md` — personalidad global de la instancia (tono, créditos). Es lo más parecido a una "regla global" persistente en Hermes; Hermes ya siembra un `SOUL.md` inicial, así que esto lo sobrescribe con el tono de Team Pinky.
- `~/.hermes/skills/` — carpeta única de skills (fuente de la verdad). Todas las personas y reglas de ingeniería migradas viven aquí como skills invocables con `/nombre`.
- Hermes también soporta compartir skills entre herramientas vía `~/.agents/skills/` (configurando `skills.external_dirs` en `~/.hermes/config.yaml`), si prefieres una única carpeta de skills reusada por Copilot/Claude/Hermes.

Hermes carga `AGENTS.md` desde el **directorio de trabajo actual** al iniciar sesión (no desde `~/.hermes`). Los skills de proyecto en `.hermes/skills/` o `.agents/skills/` requieren `hermes skills trust` la primera vez que se detectan, y tienen precedencia sobre skills globales del mismo nombre.

## Dónde vive esto globalmente en Hermes

| Alcance | Ruta | Formato |
| :--- | :--- | :--- |
| Personalidad global (lo más cercano a "regla global") | `~/.hermes/SOUL.md` | Markdown plano |
| Hechos/memoria global | `~/.hermes/MEMORY.md`, `~/.hermes/USER.md` | Markdown plano, tamaño acotado (~2200/~1375 caracteres) |
| Contexto de proyecto (por repo, no global) | `AGENTS.md` en el cwd (también lee `.cursorrules`) | Markdown plano |
| Skills (única unidad de extensión — agentes y reglas incluidos) | `~/.hermes/skills/` (global) / `<repo>/.hermes/skills/` o `<repo>/.agents/skills/` (proyecto, requiere trust) | Carpeta con `SKILL.md` (agentskills.io) |
| Compartir skills entre herramientas | `skills.external_dirs` en `~/.hermes/config.yaml` (ej. apuntando a `~/.agents/skills/`) | Lista de rutas |
| Config general | `~/.hermes/config.yaml` | YAML |

No existe un "`AGENTS.md` global" en Hermes: solo se lee desde el directorio de trabajo. Para lograr un efecto "siempre activo" sin importar el proyecto, la vía soportada es `SOUL.md` (personalidad) + skills globales en `~/.hermes/skills/`.
