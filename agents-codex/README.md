# agents-codex

- `~/.codex/AGENTS.md` — Codex reads this **always**, in any project (global level, highest discovery priority along with `AGENTS.override.md`).
- `~/.codex/agents/*.toml` — custom agents (subagents) available in any session.
- Skills: Codex does not document its own fixed user folder; they are referenced by explicit path in `skills.config` inside `config.toml`/agents, or shared via the cross-tool directory `~/.agents/skills/` (same used by Claude/Copilot/Hermes).

## Where this lives globally in Codex

| Scope | Path | Format |
| :--- | :--- | :--- |
| "Always active" instructions (user/global) | `~/.codex/AGENTS.md` (or `AGENTS.override.md` for a temporary override) | Plain Markdown, concatenated with project ones |
| "Always active" instructions (repo) | `AGENTS.md` at repo root and subfolders | Plain Markdown |
| Behavior configuration (model, sandbox, approvals) | `~/.codex/config.toml` (global) / `.codex/config.toml` (project) | TOML |
| Custom agents (subagents) | `~/.codex/agents/*.toml` (user) / `.codex/agents/*.toml` (project) | TOML (`name`, `description`, `developer_instructions`) |
| Command permission rules (not "coding rules") | `~/.codex/rules/*.rules` (user) / `<repo>/.codex/rules/*.rules` (project, only if the project is trusted) | Starlark |
| Skills | No fixed user folder documented by Codex; referenced by path in `skills.config`, commonly sharing `~/.agents/skills/` with other tools | Folder with `SKILL.md` (agentskills.io) |

There is no separate "global `AGENTS.md`" with a different name: it is literally `AGENTS.md`, just located in `~/.codex/` instead of a repo root.
