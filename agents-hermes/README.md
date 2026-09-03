# agents-hermes

- `~/.hermes/SOUL.md` — Global instance personality (tone, credits). This is the closest thing to a persistent "global rule" in Hermes; Hermes already seeds an initial `SOUL.md`, so this overrides it with Team Pinky's tone.
- `~/.hermes/skills/` — Single skills folder (source of truth). All migrated personas and engineering rules live here as skills invocable with `/name`.
- Hermes also supports sharing skills across tools via `~/.agents/skills/` (configuring `skills.external_dirs` in `~/.hermes/config.yaml`), if you prefer a single skills folder shared by Copilot/Claude/Hermes.

Hermes loads `AGENTS.md` from the **current working directory** at session start (not from `~/.hermes`). Project skills in `.hermes/skills/` or `.agents/skills/` require `hermes skills trust` the first time they are detected, and take precedence over global skills with the same name.

## Where this lives globally in Hermes

| Scope | Path | Format |
| :--- | :--- | :--- |
| Global personality (closest to "global rule") | `~/.hermes/SOUL.md` | Plain Markdown |
| Global facts/memory | `~/.hermes/MEMORY.md`, `~/.hermes/USER.md` | Plain Markdown, bounded size (~2200/~1375 characters) |
| Project context (per repo, not global) | `AGENTS.md` in cwd (also reads `.cursorrules`) | Plain Markdown |
| Skills (single extension unit — agents and rules included) | `~/.hermes/skills/` (global) / `<repo>/.hermes/skills/` or `<repo>/.agents/skills/` (project, requires trust) | Folder with `SKILL.md` (agentskills.io) |
| Sharing skills across tools | `skills.external_dirs` in `~/.hermes/config.yaml` (e.g. pointing to `~/.agents/skills/`) | List of paths |
| General config | `~/.hermes/config.yaml` | YAML |

There is no "global `AGENTS.md`" in Hermes: it is only read from the working directory. To achieve a "always active" effect regardless of the project, the supported path is `SOUL.md` (personality) + global skills in `~/.hermes/skills/`.
