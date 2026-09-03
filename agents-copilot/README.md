# agents-copilot

Adaptación de [`agents-antigravity`](../agents-antigravity) al esquema de personalización de **GitHub Copilot** (VS Code / Agent Host / GitHub.com).

## Cómo instalar en un repositorio

Copia el contenido de `.github/` de esta carpeta directamente a la raíz de tu repositorio:

```bash
cp -r agents-copilot/.github /ruta/a/tu/repo/
```

Resultado esperado en el repo destino:

```
.github/
  copilot-instructions.md      # Siempre activo (equivalente a AGENTS.md/GEMINi.md)
  instructions/*.instructions.md   # Reglas por archivo (applyTo glob) — de rules/*.rules.md
  agents/*.agent.md                # Personas/orquestador — de skills/<persona>/SKILL.md
  skills/<name>/SKILL.md            # Capacidades portables — copiadas tal cual
```

## Tabla de equivalencias (antigravity → Copilot)

| Antigravity | GitHub Copilot | Notas |
| :--- | :--- | :--- |
| `AGENTS.md` / `GEMINi.md` (raíz) | `.github/copilot-instructions.md` | Siempre se inyecta en cada request del workspace |
| `rules/*.rules.md` | `.github/instructions/*.instructions.md` | Se quitó el campo `trigger`; se mantuvo `description` y `applyTo` |
| `skills/<persona>/SKILL.md` (roles/orquestador) | `.github/agents/<persona>.agent.md` | Frontmatter cambiado a `name` + `description` + `tools` + `argument-hint` |
| `skills/<tema>/SKILL.md` (capacidad técnica portable) | `.github/skills/<tema>/SKILL.md` | Copiado sin cambios (formato ya compatible, estándar agentskills.io) |
| Trigger `/comando` | Selector de agente (`@nombre-agente`) o slash-command de skill (`/nombre-skill`) | Copilot no soporta triggers de texto libre para agentes, solo para skills/prompts |

## Hallazgos durante la migración

- `skills/agent-memory/SKILL.md` es un duplicado de `skills/cogni/SKILL.md` con `name: cogni` pero carpeta `agent-memory` (inconsistencia ya presente en el origen). No se migró para evitar conflicto de `name` vs. directorio (regla dura de Copilot: deben coincidir).
- `saul-goodman` referenciaba `skills/branding-analysis/SKILL.md`, que no existe en el repo origen. Se dejó una nota en el `.agent.md` migrado.
- `voice.rules.md` depende del MCP `pocket-tts`, no estándar en Copilot. No se migró; ver nota en `copilot-instructions.md`.

## Dónde vive esto globalmente en GitHub Copilot (Linux)

Ver la respuesta completa en la conversación, resumen rápido:

| Alcance | Ruta | Formato |
| :--- | :--- | :--- |
| Instrucciones siempre activas (repo) | `.github/copilot-instructions.md` o `AGENTS.md` en la raíz | Markdown plano |
| Instrucciones por archivo (repo) | `.github/instructions/*.instructions.md` | Frontmatter `applyTo` |
| Instrucciones por archivo (usuario, VS Code UI) | Carpeta de perfil de VS Code (`chat.instructionsFilesLocations`) | `.instructions.md` |
| Instrucciones por archivo (usuario, Agent Host / harness-agnóstico) | `~/.copilot/instructions/` | `.instructions.md` |
| Agentes personalizados (repo) | `.github/agents/*.agent.md` | Frontmatter + cuerpo |
| Agentes personalizados (usuario, Agent Host) | `~/.copilot/agents/` | `.agent.md` |
| Skills de agente (repo) | `.github/skills/`, `.claude/skills/`, `.agents/skills/` | Carpeta con `SKILL.md` |
| Skills de agente (usuario, cualquier harness) | `~/.copilot/skills/`, `~/.claude/skills/`, `~/.agents/skills/` | Carpeta con `SKILL.md` |
| Prompt files (solo VS Code, no Agent Host) | `.github/prompts/*.prompt.md` (repo) / perfil de VS Code (usuario) | `.prompt.md` |
