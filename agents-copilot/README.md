# agents-copilot

Esquema de personalización de **GitHub Copilot** (VS Code / Agent Host / GitHub.com).

Copilot tiene **dos alcances independientes** para este mismo contenido, y no se excluyen entre sí — puedes tener ambos activos a la vez:

| Alcance | ¿Necesita `.github/`? | ¿Dónde vive? | ¿A qué repos aplica? |
| :--- | :--- | :--- | :--- |
| **Repositorio** | **Sí, obligatorio.** Copilot solo detecta `copilot-instructions.md` / `instructions/` / `agents/` / `skills/` si viven dentro de `.github/` en la raíz de ESE repo. | Dentro del propio repo (versionado en git) | Solo ese repo |
| **Usuario / global (Agent Host)** | **No.** No lleva wrapper `.github/` porque no vive en ningún repo; vive en tu `$HOME`. | `~/.copilot/instructions/`, `~/.copilot/agents/`, `~/.copilot/skills/` | **Todos** los repos que abras (CLI, cloud agent, y VS Code si usa Agent Host) |

Es decir: si solo quieres reglas **globales** (tu caso), NO necesitas tocar `.github/` para nada — solo necesitas los archivos en `~/.copilot/...`, que ya está hecho en este equipo (ver sección más abajo). El wrapper `.github/` solo es necesario si además quieres versionar estas reglas dentro de un repositorio específico para compartirlas con tu equipo vía git.

Resultado esperado en el repo destino:

```md
.github/
  copilot-instructions.md      # Siempre activo (equivalente a AGENTS.md/GEMINi.md)
  instructions/*.instructions.md   # Reglas por archivo (applyTo glob) — de rules/*.rules.md
  agents/*.agent.md                # Personas/orquestador — de skills/<persona>/SKILL.md
  skills/<name>/SKILL.md            # Capacidades portables — copiadas tal cual
```

Nota clave: existen **dos "perfiles de usuario" distintos** y no comparten carpeta:

- El perfil de VS Code (`$VSCODE_USER_PROMPTS_FOLDER`) es propio de la UI de VS Code; útil si usas Copilot Chat dentro del editor, pero **invisible** para sesiones de Agent Host (CLI `copilot`, cloud agent).
- `~/.copilot/` es el perfil harness-agnóstico; lo lee tanto el Agent Host como VS Code cuando corre sobre Agent Host. Para reglas verdaderamente globales (tu caso), esta es la carpeta correcta — y es la que ya está poblada en este equipo.

**No existe un único "`AGENTS.md` global"** en Copilot: no hay un nombre de archivo fijo obligatorio a nivel usuario. Lo que lo hace "global" es la ubicación (`~/.copilot/instructions/`) más `applyTo: '**'` en el frontmatter — el nombre del archivo es libre.

Prioridad cuando coexisten instrucciones globales (usuario) y de repo (`.github/copilot-instructions.md` o `AGENTS.md`): las de usuario ganan en tono/estilo, las de repo ganan en convenciones específicas del proyecto — ver [instruction priority](https://code.visualstudio.com/docs/agent-customization/custom-instructions#_instruction-priority).
