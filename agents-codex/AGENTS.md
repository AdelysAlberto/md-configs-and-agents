# Team Pinky - Codex Core Registry & Universal Invariants

> Codex concatenates `AGENTS.md` files from `~/.codex/AGENTS.md` (global) down to your current
> directory (project, then nested overrides), joined with blank lines, up to `project_doc_max_bytes`
> (32 KiB default). Keep this file lean — bulky reference material belongs in a Skill instead,
> where Codex loads it on demand via progressive disclosure.

## Response Style, Language & Tone (Universal)

- **Language**: ALWAYS respond in **Neutral Spanish**.
- **Prose Style**: Skip filler phrases. Provide code/diffs directly. Confirm file operations in 1 line.
- **Critical Thinking**: Rigorously evaluate every proposal. Challenge technical debt and complacency.
- **No Emojis Policy**: Prohibit emojis in `README.md`, docs, and reports unless explicitly requested.


- Reason exclusively in English.
- Keep reasoning terse and compressed.
- Avoid translating intermediate thoughts to Spanish.
- Only the final answer should be written in Spanish, Respond to the user in Spanish.
- Generate code, commit messages, variable names and technical analysis in English.

## Specialist Custom Agents (`~/.codex/agents/*.toml` or `.codex/agents/*.toml`)

Spawn with a direct request ("delegate to `vicky_techlead`") or let Codex trigger them per project/skill instructions.
`name` uses underscores (Codex agent-name convention); files below use hyphens to match their persona identity.

| Specialist | Role | File |
| :--- | :--- | :--- |
| `profesor_orchestrator` | Overall strategy & orchestration | [profesor-orchestrator.toml](agents/profesor-orchestrator.toml) |
| `sherlock_analyst` | Market & competitor research | [sherlock-analyst.toml](agents/sherlock-analyst.toml) |
| `roz_product` | Product requirements & PRD | [roz-product.toml](agents/roz-product.toml) |
| `edna_ux` | UX/UI design & visual system | [edna-ux.toml](agents/edna-ux.toml) |
| `miranda_css` | CSS Modules, BEM & tokens | [miranda-css.toml](agents/miranda-css.toml) |
| `sheldon_architect` | System architecture, DDL & APIs | [sheldon-architect.toml](agents/sheldon-architect.toml) |
| `doc_database` | Database, ORM, Redis & indexes | [doc-database.toml](agents/doc-database.toml) |
| `gorgory_security` | Security, OWASP & API shielding | [gorgory-security.toml](agents/gorgory-security.toml) |
| `vicky_techlead` | Clean Architecture & Scaffolding | [vicky-techlead.toml](agents/vicky-techlead.toml) |
| `house_testing` | Unit, integration & MSW tests | [house-testing.toml](agents/house-testing.toml) |
| `gadget_auditor` | Dead code & API discrepancies | [gadget-auditor.toml](agents/gadget-auditor.toml) |
| `tio_bob` | Code reviewer for MR/PR and staged changes | [tio-bob.toml](agents/tio-bob.toml) |
| `monk_scrum` | Scrum Epics & step-by-step tasks | [monk-scrum.toml](agents/monk-scrum.toml) |
| `paul_finch` | Technical writer & documentation | [paul-finch.toml](agents/paul-finch.toml) |
| `readme` | README designer & GitHub layout | [readme.toml](agents/readme.toml) |
| `saul_goodman` | Marketing & branding analyst | [saul-goodman.toml](agents/saul-goodman.toml) |

## Engineering Standards & Language Rules (`skills/*/SKILL.md`)

Codex has no `applyTo` glob mechanism like Copilot. The 19 antigravity `rules/*.rules.md` files were
converted into **Skills** instead — Codex loads a skill's full body only when its `description`
(which embeds the original `applyTo` glob as text) matches the current task, via `$skill-name` or
automatic relevance matching. Always-relevant ones (`engineering-invariants`, `runtime`,
`verification-checklist`, `commits`, `cogni`) should still be invoked explicitly at the start of
non-trivial tasks since Codex does not auto-apply skills by file glob.

Portable capability skills (`astro`, `bun`, `cloudflare`, `react-native-architecture`,
`react-typescript-clean-code`, `zustand`, `frontend-design`, `commit`, `cogni`) are unchanged from
the antigravity source (already `agentskills.io`-compliant).

## Command Permission Rules — NOT the same as coding-standard rules

Codex's own `rules/*.rules` (Starlark, under `~/.codex/rules/`) control **shell command
approval/sandbox policy** (`prefix_rule(pattern=..., decision="allow"|"prompt"|"forbidden")`), not
coding conventions. Do not confuse them with the antigravity `rules/*.rules.md` coding-standard
files — those were migrated to Skills above, not to Codex `.rules` files.
