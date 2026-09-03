# Team Pinky - Project Context for Hermes Agent

> Hermes loads the top-level `AGENTS.md` from the **current working directory** at session
> start (not from `~/.hermes`). Place a copy of this file at the root of each project where you
> want Team Pinky conventions active, or launch `hermes` from a folder that has one. Subdirectory
> `AGENTS.md` files are discovered lazily and injected into tool results.

## Working Agreements

- Always respond in Neutral Latin American Spanish.
- No emojis in README.md, docs, or reports unless explicitly requested.
- Include a Credits/About section in built apps: Adelys Alberto Belen — Software Engineer — adalbeca.com — <dev@adalbeca.com>.
- Before non-trivial bugfixes or architecture decisions, invoke the `cogni` skill to search/save memory signatures.
Always perform internal reasoning, planning, analysis, and chain-of-thought in English.

Use English for: reasoning / planning / task decomposition / code analysis /architecture analysis
Use Spanish only for: final user-facing responses / comments that are intended for the user / documentation when explicitly requested

Keep internal reasoning concise and token-efficient.

## Specialists available as Skills

Hermes has no lightweight "custom agent" file format like Copilot's `.agent.md` or Codex's agent
`.toml` — skills are the only extensibility unit. Each Team Pinky persona below is a Skill invoked
with `/persona-name`, and its instructions ask the agent to **adopt that persona** for the turn:
`profesor-orchestrator`, `sherlock-analyst`, `roz-product`, `edna-ux`, `miranda-css`,
`sheldon-architect`, `doc-database`, `gorgory-security`, `vicky-techlead`, `house-testing`,
`gadget-auditor`, `tio-bob`, `monk-scrum`, `paul-finch`, `readme`, `saul-goodman`.

## Engineering standards as Skills

The antigravity `rules/*.rules.md` files (architecture, coding-standards, testing, styling, etc.)
were converted into Skills — Hermes has no per-file `applyTo` glob system, so these load on-demand
via the skill's `description` (semantic match) or explicit `/skill-name` invocation.

See [README.md](README.md) for the full install mapping and global paths.
