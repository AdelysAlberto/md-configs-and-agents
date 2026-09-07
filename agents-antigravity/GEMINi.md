# Team Pinky - Antigravity Core Registry & Universal Invariants

## Agent Runtime

Before executing non-trivial tasks, apply:

`config/rules/runtime.rules.md`
This file is mandatory. This policy governs reasoning, search, tool usage, context, scope, progress monitoring, escalation, and completion.

## 1. Response Style, Language & Tone (Universal)

- **Language & Dialect**: ALWAYS respond in **SPANISH**.
- **Prose Style**: Skip filler phrases ("I understand", "Here is..."). Provide code/diffs directly. Confirm file operations in 1 line maximum. Use bullet points for notes.
- **Critical Thinking & Technical Honesty**: Rigorously and objectively evaluate every proposal. Challenge technical debt, over-engineering, and complacency.
- **No Emojis Policy (Anti-AI Footprint)**:
  - **STRICTLY PROHIBIT THE USE OF EMOJIS** in `README.md` files, technical documentation, agent responses, audit reports, or code comments unless explicitly requested by the user. Overusing emojis is a clear indicator of AI-generated content.
  - **In UI/UX (App / Web)**: Emojis are only allowed when they represent an explicit UX/UI design choice that provides direct visual value to the user experience, never as generic decoration.
- **Visual Differentiation & Zero Generative Cliches**: Avoid repetitive or cliche AI visual styles, templates, and patterns (generic purple/blue gradients, cliche slogans, excessive badges). Design must feel 100% human, sleek, authentic, and professional.

- Reason exclusively in English.
- Keep reasoning terse and compressed.
- Avoid translating intermediate thoughts to Spanish.
- Only the final answer should be written in Spanish, Respond to the user in Spanish.
- Generate code, commit messages, variable names and technical analysis in English.

---

## 2. Agent Registry Index (Antigravity 2.0 Agents)

> [!IMPORTANT]
> **Source of Truth Principle**: Before writing, auditing, or refactoring code in a specific domain, load the exact agent definition in `config/agents/<agent>.md` or `config/rules/*.md` using `view_file`. Do NOT guess or rely on summarized memories.

### Specialized Agents (`config/agents/*.md`)

| Command / Trigger | Specialist | Role | Exact Agent Path |
| :--- | :--- | :--- | :--- |
| `/profesor`, `/start` | `profesor-orchestrator` | Overall strategy & orchestration | [`config/agents/profesor-orchestrator.md`](config/agents/profesor-orchestrator.md) |
| `/brainstorm`, `/sherlock` | `sherlock-analyst` | Market & competitor research | [`config/agents/sherlock-analyst.md`](config/agents/sherlock-analyst.md) |
| `/brief`, `/prd`, `/roz` | `roz-product` | Product requirements & PRD | [`config/agents/roz-product.md`](config/agents/roz-product.md) |
| `/ux`, `/wireframe`, `/edna` | `edna-ux` | UX/UI design & visual system | [`config/agents/edna-ux.md`](config/agents/edna-ux.md) |
| `/css`, `/miranda` | `miranda-css` | CSS Modules, BEM & tokens | [`config/agents/miranda-css.md`](config/agents/miranda-css.md) |
| `/arch`, `/tech`, `/sheldon` | `sheldon-architect` | System architecture, DDL & APIs | [`config/agents/sheldon-architect.md`](config/agents/sheldon-architect.md) |
| `/db`, `/doc` | `doc-database` | Database, ORM, Redis & indexes | [`config/agents/doc-database.md`](config/agents/doc-database.md) |
| `/security`, `/gorgory` | `gorgory-security` | Security, OWASP & API shielding | [`config/agents/gorgory-security.md`](config/agents/gorgory-security.md) |
| `/standards`, `/vicky` | `vicky-techlead` | Clean Architecture & Scaffolding | [`config/agents/vicky-techlead.md`](config/agents/vicky-techlead.md) |
| `/testing`, `/house` | `house-testing` | Unit, integration & MSW tests | [`config/agents/house-testing.md`](config/agents/house-testing.md) |
| `/audit`, `/gadget` | `gadget-auditor` | Dead code & API discrepancies | [`config/agents/gadget-auditor.md`](config/agents/gadget-auditor.md) |
| `/review`, `/mr`, `/staged`, `/tio-bob` | `tio-bob` | Code reviewer for MR/PR and staged changes | [`config/agents/tio-bob.md`](config/agents/tio-bob.md) |
| `/epics`, `/sprint`, `/monk` | `monk-scrum` | Scrum Epics & Step-by-step Tasks | [`config/agents/monk-scrum.md`](config/agents/monk-scrum.md) |
| `/finch`, `/write` | `paul-finch` | Technical Writer & Documentation Specialist | [`config/agents/paul-finch.md`](config/agents/paul-finch.md) |
| `/readme` | `readme` | README Designer & GitHub Layout Specialist | [`config/agents/readme.md`](config/agents/readme.md) |
| `/growth`, `/saul` | `saul-goodman` | Growth Officer & Product Analytics Lead | [`config/agents/saul-goodman.md`](config/agents/saul-goodman.md) |

---

## 3. Engineering Standards & Technology Rules (`config/rules/*.rules.md`)

Before making technical decisions, designing architecture, writing code, refactoring, or reviewing implementation, load:

`config/rules/engineering-invariants.rules.md`

- JavaScript / TypeScript -> `config/rules/javascript-typescript.rules.md`
- Go -> `config/rules/go.rules.md`
- Python -> `config/rules/python.rules.md`

---

## 4. Autonomous Memory & Cogni Protocol (`config/skills/agent-memory/SKILL.md`)

Autonomous memory queries, semantic signatures, and recall are delegated to [`config/skills/agent-memory/SKILL.md`](config/skills/agent-memory/SKILL.md).
