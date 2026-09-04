# Team Pinky - GitHub Copilot Core Registry & Universal Invariants

## Agent Runtime

Before executing non-trivial tasks, apply the rules in [runtime.instructions.md](instructions/runtime.instructions.md).
This policy governs reasoning, search, tool usage, context, scope, progress monitoring, escalation, and completion.

## 💬 1. Response Style, Language & Tone (Universal)

- **Language & Dialect**: ALWAYS respond in **Neutral Latin American Spanish** (no *"vosotros"*, *"hacéis"*, *"decís"*).
- **Prose Style**: Skip filler phrases (*"I understand"*, *"Here is..."*). Provide code/diffs directly. Confirm file operations in 1 line maximum. Use bullet points for notes.
- **Critical Thinking & Technical Honesty**: Rigorously and objectively evaluate every proposal. Challenge technical debt, over-engineering, and complacency.
- **No Emojis Policy (Anti-AI Footprint)**:
  - **STRICTLY PROHIBIT THE USE OF EMOJIS** in `README.md` files, technical documentation, agent responses, audit reports, or code comments unless explicitly requested by the user.
  - **In UI/UX (App / Web)**: Emojis are only allowed when they represent an explicit UX/UI design choice that provides direct visual value to the user experience, never as generic decoration.
- **Visual Differentiation & Zero Generative Clichés**: Avoid repetitive or cliché AI visual styles, templates, and patterns (generic purple/blue gradients, cliché slogans, excessive badges).


- Reason exclusively in English.
- Keep reasoning terse and compressed.
- Avoid translating intermediate thoughts to Spanish.
- Only the final answer should be written in Spanish, Respond to the user in Spanish.
- Generate code, commit messages, variable names and technical analysis in English.


---

## 🧭 2. Agent & Skill Registry (Lazy Loading)

> [!IMPORTANT]
> **Source of Truth Principle**: Before writing, auditing, or refactoring code in a specific domain, open the exact `.agent.md` / `SKILL.md` / `*.instructions.md` file. Do NOT guess or rely on summarized memories.

### Specialized Custom Agents (`.github/agents/*.agent.md`)

Invoke by name in the agent picker, or ask the orchestrator to delegate. `argument-hint` shows the legacy antigravity trigger words for reference (Copilot does not use `/command` triggers for agents — use the Agents dropdown or `@agent-name` instead).

| Specialist | Role | File |
| :--- | :--- | :--- |
| `profesor-orchestrator` | Overall strategy & orchestration | [profesor-orchestrator.agent.md](agents/profesor-orchestrator.agent.md) |
| `sherlock-analyst` | Market & competitor research | [sherlock-analyst.agent.md](agents/sherlock-analyst.agent.md) |
| `roz-product` | Product requirements & PRD | [roz-product.agent.md](agents/roz-product.agent.md) |
| `edna-ux` | UX/UI design & visual system | [edna-ux.agent.md](agents/edna-ux.agent.md) |
| `miranda-css` | CSS Modules, BEM & tokens | [miranda-css.agent.md](agents/miranda-css.agent.md) |
| `sheldon-architect` | System architecture, DDL & APIs | [sheldon-architect.agent.md](agents/sheldon-architect.agent.md) |
| `doc-database` | Database, ORM, Redis & indexes | [doc-database.agent.md](agents/doc-database.agent.md) |
| `gorgory-security` | Security, OWASP & API shielding | [gorgory-security.agent.md](agents/gorgory-security.agent.md) |
| `vicky-techlead` | Clean Architecture & Scaffolding | [vicky-techlead.agent.md](agents/vicky-techlead.agent.md) |
| `house-testing` | Unit, integration & MSW tests | [house-testing.agent.md](agents/house-testing.agent.md) |
| `gadget-auditor` | Dead code & API discrepancies | [gadget-auditor.agent.md](agents/gadget-auditor.agent.md) |
| `tio-bob` | Code reviewer for MR/PR and staged changes | [tio-bob.agent.md](agents/tio-bob.agent.md) |
| `monk-scrum` | Scrum Epics & step-by-step tasks | [monk-scrum.agent.md](agents/monk-scrum.agent.md) |
| `paul-finch` | Technical writer & documentation | [paul-finch.agent.md](agents/paul-finch.agent.md) |
| `readme` | README designer & GitHub layout | [readme.agent.md](agents/readme.agent.md) |
| `saul-goodman` | Marketing & branding analyst | [saul-goodman.agent.md](agents/saul-goodman.agent.md) |

### Portable Agent Skills (`.github/skills/*/SKILL.md`)

Loaded automatically when relevant, or invoked as `/skill-name` in chat.

| Skill | Purpose | File |
| :--- | :--- | :--- |
| `cogni` | Autonomous local memory (SQLite) | [cogni/SKILL.md](skills/cogni/SKILL.md) |
| `commit` | Conventional commit generation from git diff | [commit/SKILL.md](skills/commit/SKILL.md) |
| `astro` | Astro framework guidance | [astro/SKILL.md](skills/astro/SKILL.md) |
| `bun` | Bun runtime/toolchain guidance | [bun/SKILL.md](skills/bun/SKILL.md) |
| `cloudflare` | Cloudflare platform guidance | [cloudflare/SKILL.md](skills/cloudflare/SKILL.md) |
| `react-native-architecture` | React Native app architecture | [react-native-architecture/SKILL.md](skills/react-native-architecture/SKILL.md) |
| `react-typescript-clean-code` | React + TypeScript engineering standards | [react-typescript-clean-code/SKILL.md](skills/react-typescript-clean-code/SKILL.md) |
| `zustand` | Zustand state management patterns | [zustand/SKILL.md](skills/zustand/SKILL.md) |
| `frontend-design` | Distinctive visual/UI design direction | [frontend-design/SKILL.md](skills/frontend-design/SKILL.md) |

> Note: the antigravity `agent-memory` skill was a duplicate of `cogni` (identical content, mismatched folder/`name`) and was intentionally **not** migrated. The `branding-analysis` skill referenced by `saul-goodman` did not exist in the source repository either — flag if you need it recreated.

## 3. Engineering Standards

Before making technical decisions, designing architecture, writing code, refactoring, or reviewing implementation, load [engineering-invariants.instructions.md](instructions/engineering-invariants.instructions.md).

These invariants define the universal engineering standards that apply regardless of programming language, framework, or technology. `applyTo` glob patterns below make most of this automatic in Copilot — you rarely need to load these manually.

## 4. Language & Technology Instructions (`.github/instructions/*.instructions.md`)

Copilot applies these automatically based on the `applyTo` glob of the file you're editing. Load manually only when working outside those globs (e.g. planning docs).

| File | Applies to |
| :--- | :--- |
| [architecture.instructions.md](instructions/architecture.instructions.md) | `src/**` |
| [backend_workflow_standards.instructions.md](instructions/backend_workflow_standards.instructions.md) | `src/**/*.ts`, `src/providers/**`, `src/modules/**` |
| [coding-standards.instructions.md](instructions/coding-standards.instructions.md) | `src/**/*.ts`, `src/**/*.tsx` |
| [cogni.instructions.md](instructions/cogni.instructions.md) | `**` |
| [commits.instructions.md](instructions/commits.instructions.md) | `**` |
| [components.instructions.md](instructions/components.instructions.md) | `src/baseComponents/**`, `src/pages/**`, `src/providers/**`, `src/components/**` |
| [config-setup.instructions.md](instructions/config-setup.instructions.md) | `*.json`, `*.ts`, config files |
| [engineering-invariants.instructions.md](instructions/engineering-invariants.instructions.md) | `src/**` |
| [go.instructions.md](instructions/go.instructions.md) | `**/*.go` |
| [i18n.instructions.md](instructions/i18n.instructions.md) | `src/**/*.ts`, `src/**/*.tsx`, `src/providers/lang/**` |
| [javascript-typescript.instructions.md](instructions/javascript-typescript.instructions.md) | `**/*.tsx`, `**/*.ts`, `**/*.js`, `**/*.jsx` |
| [reactjs.instructions.md](instructions/reactjs.instructions.md) | `**/*.tsx`, `**/*.ts`, `**/*.js`, `**/*.jsx` |
| [runtime.instructions.md](instructions/runtime.instructions.md) | `src/**` |
| [services-hooks.instructions.md](instructions/services-hooks.instructions.md) | `src/**/services/**`, `src/**/hooks/**` |
| [state-management.instructions.md](instructions/state-management.instructions.md) | `src/**/state/**`, `src/**/store/**` |
| [styling.instructions.md](instructions/styling.instructions.md) | `src/**/*.css`, `src/**/*.scss`, `src/styles/**` |
| [testing.instructions.md](instructions/testing.instructions.md) | `src/**/__tests__/**`, `*.test.ts(x)` |
| [ui-library.instructions.md](instructions/ui-library.instructions.md) | `src/**/*.tsx`, `src/components/**/*` |
| [verification-checklist.instructions.md](instructions/verification-checklist.instructions.md) | `**` |

> `voice.rules.md` (Pocket-TTS MCP trigger) was intentionally **not** migrated — it depends on an MCP tool not part of the standard Copilot toolset. Recreate it as a `.github/agents/*.agent.md` with `tools: ['pocket-tts/*']` if you install that MCP server.

## 5. Autonomous Memory & Cogni Protocol

Autonomous memory queries, semantic signatures, and recall are delegated to [cogni/SKILL.md](skills/cogni/SKILL.md) and [cogni.instructions.md](instructions/cogni.instructions.md).
