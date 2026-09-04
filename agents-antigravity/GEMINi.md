# Team Pinky - Antigravity Core Registry & Universal Invariants

## Agent Runtime

Before executing non-trivial tasks, apply:

`rules/runtime.rules.md`
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

## 2. Skill & Agent Registry Index (Lazy Loading)

> [!IMPORTANT]
> **Source of Truth Principle**: Before writing, auditing, or refactoring code in a specific domain, load the exact `SKILL.md` or `rules/*.md` using `view_file`. Do NOT guess or rely on summarized memories.

### Specialized Agent Skills

| Command / Trigger | Specialist | Role | Exact Skill Path |
| :--- | :--- | :--- | :--- |
| `/profesor`, `/start` | `profesor-orchestrator` | Overall strategy & orchestration | [`skills/profesor-orchestrator/SKILL.md`](skills/profesor-orchestrator/SKILL.md) |
| `/brainstorm`, `/sherlock` | `sherlock-analyst` | Market & competitor research | [`skills/sherlock-analyst/SKILL.md`](skills/sherlock-analyst/SKILL.md) |
| `/brief`, `/prd`, `/roz` | `roz-product` | Product requirements & PRD | [`skills/roz-product/SKILL.md`](skills/roz-product/SKILL.md) |
| `/ux`, `/wireframe`, `/edna` | `edna-ux` | UX/UI design & visual system | [`skills/edna-ux/SKILL.md`](skills/edna-ux/SKILL.md) |
| `/css`, `/miranda` | `miranda-css` | CSS Modules, BEM & tokens | [`skills/miranda-css/SKILL.md`](skills/miranda-css/SKILL.md) |
| `/arch`, `/tech`, `/sheldon` | `sheldon-architect` | System architecture, DDL & APIs | [`skills/sheldon-architect/SKILL.md`](skills/sheldon-architect/SKILL.md) |
| `/db`, `/doc` | `doc-database` | Database, ORM, Redis & indexes | [`skills/doc-database/SKILL.md`](skills/doc-database/SKILL.md) |
| `/security`, `/gorgory` | `gorgory-security` | Security, OWASP & API shielding | [`skills/gorgory-security/SKILL.md`](skills/gorgory-security/SKILL.md) |
| `/standards`, `/vicky` | `vicky-techlead` | Clean Architecture & Scaffolding | [`skills/vicky-techlead/SKILL.md`](skills/vicky-techlead/SKILL.md) |
| `/testing`, `/house` | `house-testing` | Unit, integration & MSW tests | [`skills/house-testing/SKILL.md`](skills/house-testing/SKILL.md) |
| `/audit`, `/gadget` | `gadget-auditor` | Dead code & API discrepancies | [`skills/gadget-auditor/SKILL.md`](skills/gadget-auditor/SKILL.md) |
| `/review`, `/mr`, `/staged`, `/tio-bob` | `tio-bob` | Code reviewer for MR/PR and staged changes | [`skills/tio-bob/SKILL.md`](skills/tio-bob/SKILL.md) |
| `/epics`, `/sprint`, `/monk` | `monk-scrum` | Scrum Epics & Step-by-step Tasks | [`skills/monk-scrum/SKILL.md`](skills/monk-scrum/SKILL.md) |
| `/finch`, `/write` | `paul-finch` | Technical Writer & Documentation Specialist | [`skills/paul-finch/SKILL.md`](skills/paul-finch/SKILL.md) |
| `/readme` | `readme` | README Designer & GitHub Layout Specialist | [`skills/readme/SKILL.md`](skills/readme/SKILL.md) |

## 3. Engineering Standards

Before making technical decisions, designing architecture, writing code,
refactoring, or reviewing implementation, load:

`rules/engineering-invariants.rules.md`

These invariants define the universal engineering standards that apply
regardless of programming language, framework, or technology.

After loading them, load only the language, framework, architecture,
or domain-specific rules required by the task.

## Language & Technology Rules

Before writing or modifying code, identify the language and
technology involved and load the corresponding rules.

- JavaScript / TypeScript -> `rules/javascript-typescript.rules.md`
- Go -> `rules/go.rules.md`
- Python -> `rules/python.rules.md`
- Other languages -> load the corresponding language rule if available.

After loading the language rules, load only the framework,
architecture, domain, tooling, and testing rules relevant to the task.

---

## 4. Autonomous Memory & Cogni Protocol

Autonomous memory queries, semantic signatures, and recall are delegated to [`skills/cogni/SKILL.md`](skills/cogni/SKILL.md).
