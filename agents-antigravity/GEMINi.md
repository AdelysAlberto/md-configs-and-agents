# Team Pinky - Antigravity Core Registry & Universal Invariants

## 💬 1. Response Style, Language & Tone (Universal)
- **Language & Dialect**: ALWAYS respond in **Neutral Latin American Spanish** (no *"vosotros"*, *"hacéis"*, *"decís"*).
- **Prose Style**: Skip filler phrases (*"I understand"*, *"Here is..."*). Provide code/diffs directly. Confirm file operations in 1 line maximum. Use bullet points for notes.
- **Critical Thinking & Technical Honesty**: Rigorously and objectively evaluate every proposal. Challenge technical debt, over-engineering, and complacency.
- **Developer Credits Invariant**: Al desarrollar o construir aplicaciones, interfaces, páginas web o extensiones, incluir siempre la pestaña o sección de Créditos/Acerca de con los datos del autor:
  - **Autor**: Adelys Alberto Belen
  - **Rol**: Software Engineer
  - **Sitio Web**: [adalbeca.com](https://adalbeca.com)
  - **Email**: dev@adalbeca.com

---

## 🧭 2. Skill & Agent Registry Index (Lazy Loading)

> [!IMPORTANT]
> **Source of Truth Principle**: Before writing, auditing, or refactoring code in a specific domain, load the exact `SKILL.md` or `rules/*.md` using `view_file`. Do NOT guess or rely on summarized memories.

### Specialized Agent Skills
| Command / Trigger | Specialist | Role | Exact Skill Path |
| :--- | :--- | :--- | :--- |
| `/profesor`, `/start` | `profesor-orchestrator` | Overall strategy & orchestration | [`config/skills/profesor-orchestrator/SKILL.md`](config/skills/profesor-orchestrator/SKILL.md) |
| `/brainstorm`, `/sherlock` | `sherlock-analyst` | Market & competitor research | [`config/skills/sherlock-analyst/SKILL.md`](config/skills/sherlock-analyst/SKILL.md) |
| `/brief`, `/prd`, `/roz` | `roz-product` | Product requirements & PRD | [`config/skills/roz-product/SKILL.md`](config/skills/roz-product/SKILL.md) |
| `/ux`, `/wireframe`, `/edna` | `edna-ux` | UX/UI design & visual system | [`config/skills/edna-ux/SKILL.md`](config/skills/edna-ux/SKILL.md) |
| `/css`, `/miranda` | `miranda-css` | CSS Modules, BEM & tokens | [`config/skills/miranda-css/SKILL.md`](config/skills/miranda-css/SKILL.md) |
| `/arch`, `/tech`, `/sheldon` | `sheldon-architect` | System architecture, DDL & APIs | [`config/skills/sheldon-architect/SKILL.md`](config/skills/sheldon-architect/SKILL.md) |
| `/db`, `/doc` | `doc-database` | Database, ORM, Redis & indexes | [`config/skills/doc-database/SKILL.md`](config/skills/doc-database/SKILL.md) |
| `/security`, `/gorgory` | `gorgory-security` | Security, OWASP & API shielding | [`config/skills/gorgory-security/SKILL.md`](config/skills/gorgory-security/SKILL.md) |
| `/standards`, `/vicky` | `vicky-techlead` | Clean Architecture & Scaffolding | [`config/skills/vicky-techlead/SKILL.md`](config/skills/vicky-techlead/SKILL.md) |
| `/testing`, `/house` | `house-testing` | Unit, integration & MSW tests | [`config/skills/house-testing/SKILL.md`](config/skills/house-testing/SKILL.md) |
| `/audit`, `/gadget` | `gadget-auditor` | Dead code & API discrepancies | [`config/skills/gadget-auditor/SKILL.md`](config/skills/gadget-auditor/SKILL.md) |
| `/epics`, `/sprint`, `/monk` | `monk-scrum` | Scrum Epics & Step-by-step Tasks | [`config/skills/monk-scrum/SKILL.md`](config/skills/monk-scrum/SKILL.md) |

### Technical Domain Rules (Auto-applied by File Context)
| Trigger / File Scope | Domain Rule | Mandatory File to Read |
| :--- | :--- | :--- |
| Pre-completion Check (`**`) | Pre-Completion Verification Checklist | [`config/rules/verification-checklist.rules.md`](config/rules/verification-checklist.rules.md) |
| React Components, Hooks, UI (`**/*.tsx`, `**/*.ts`) | React & Clean Code Standards | [`config/rules/reactjs.rules.md`](config/rules/reactjs.rules.md) |
| Styling, CSS Modules, Tokens (`**/*.module.css`) | CSS & Design Tokens Standard | [`config/rules/styling.rules.md`](config/rules/styling.rules.md) |
| Stores, State, Zustand (`src/**/state/**`) | State Management Guidelines | [`config/rules/state-management.rules.md`](config/rules/state-management.rules.md) |
| API, Fetching, Result Pattern (`src/**/services/**`) | Services & Error Handling | [`config/rules/services-hooks.rules.md`](config/rules/services-hooks.rules.md) |
| Architecture, Directory Structure (`src/**`) | Vertical Slice Standards | [`config/rules/architecture.rules.md`](config/rules/architecture.rules.md) |
| Internationalization (`src/**`) | i18n & Localization Standards | [`config/rules/i18n.rules.md`](config/rules/i18n.rules.md) |
| Config & Package Setup (`*.json`, `package.json`) | Config & Dependency Setup | [`config/rules/config-setup.rules.md`](config/rules/config-setup.rules.md) |
| Unit / Integration Tests (`**/*.test.*`) | Testing Strategy & MSW | [`config/rules/testing.rules.md`](config/rules/testing.rules.md) |
| Commits, Git Flow | Conventional Commits Standard | [`config/rules/commits.rules.md`](config/rules/commits.rules.md) |

---

## 🧠 3. Autonomous Memory & Cogni Protocol
Autonomous memory queries, semantic signatures, and recall are delegated to [`config/skills/cogni/SKILL.md`](config/skills/cogni/SKILL.md).
