# agents-codex

- `~/.codex/AGENTS.md` — Codex lo lee **siempre**, en cualquier proyecto (nivel global, máxima prioridad de descubrimiento junto con `AGENTS.override.md`).
- `~/.codex/agents/*.toml` — agentes personalizados (subagentes) disponibles en cualquier sesión.
- Skills: Codex no documenta una carpeta de usuario fija propia; se referencian por ruta explícita en `skills.config` dentro de `config.toml`/agentes, o se comparten vía el directorio cross-tool `~/.agents/skills/` (mismo que usan Claude/Copilot/Hermes).

## Dónde vive esto globalmente en Codex

| Alcance | Ruta | Formato |
| :--- | :--- | :--- |
| Instrucciones "siempre activas" (usuario/global) | `~/.codex/AGENTS.md` (o `AGENTS.override.md` para un override temporal) | Markdown plano, concatenado con los de proyecto |
| Instrucciones "siempre activas" (repo) | `AGENTS.md` en la raíz del repo y subcarpetas | Markdown plano |
| Configuración de comportamiento (modelo, sandbox, aprobaciones) | `~/.codex/config.toml` (global) / `.codex/config.toml` (proyecto) | TOML |
| Agentes personalizados (subagentes) | `~/.codex/agents/*.toml` (usuario) / `.codex/agents/*.toml` (proyecto) | TOML (`name`, `description`, `developer_instructions`) |
| Reglas de permisos de comandos (no son "coding rules") | `~/.codex/rules/*.rules` (usuario) / `<repo>/.codex/rules/*.rules` (proyecto, solo si el proyecto es de confianza) | Starlark |
| Skills | Sin carpeta de usuario fija documentada propia de Codex; se referencian por ruta en `skills.config`, comúnmente compartiendo `~/.agents/skills/` con otras herramientas | Carpeta con `SKILL.md` (agentskills.io) |

No existe un "`AGENTS.md` global" con nombre distinto: es literalmente `AGENTS.md`, solo que ubicado en `~/.codex/` en vez de la raíz de un repo.
