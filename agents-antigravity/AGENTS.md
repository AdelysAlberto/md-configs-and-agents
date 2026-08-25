# Team Pinky - Antigravity Core Registry & Universal Invariants

## 💬 1. Response Style, Language & Tone (Universal)
- **Language & Dialect**: ALWAYS respond in **Neutral Latin American Spanish** (no *"vosotros"*, *"hacéis"*, *"decís"*).
- **Prose Style**: Skip filler phrases (*"I understand"*, *"Here is..."*). Provide code/diffs directly. Confirm file operations in 1 line maximum. Use bullet points for notes.
- **Critical Thinking & Technical Honesty**: Rigorously and objectively evaluate every proposal. Challenge technical debt, over-engineering, and complacency.

---

## 🧭 2. Skill & Agent Registry Index (Lazy Loading)

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
| `/review`, `/mr`, `/staged`, `/tio-bob` | `tio-bob` | Reviewer de codigo, MR/PR y cambios en stage | [`skills/tio-bob/SKILL.md`](skills/tio-bob/SKILL.md) |
| `/epics`, `/sprint`, `/monk` | `monk-scrum` | Scrum Epics & Step-by-step Tasks | [`skills/monk-scrum/SKILL.md`](skills/monk-scrum/SKILL.md) |

### Technical Domain Rules (Auto-applied by File Context)
| Trigger / File Scope | Domain Rule | Mandatory File to Read |
| :--- | :--- | :--- |
| Pre-completion Check (`**`) | Pre-Completion Verification Checklist | [`rules/verification-checklist.rules.md`](rules/verification-checklist.rules.md) |
| React Components, Hooks, UI (`**/*.tsx`, `**/*.ts`) | React & Clean Code Standards | [`rules/reactjs.rules.md`](rules/reactjs.rules.md) |
| Styling, CSS Modules, Tokens (`**/*.module.css`) | CSS & Design Tokens Standard | [`rules/styling.rules.md`](rules/styling.rules.md) |
| Stores, State, Zustand (`src/**/state/**`) | State Management Guidelines | [`rules/state-management.rules.md`](rules/state-management.rules.md) |
| API, Fetching, Result Pattern (`src/**/services/**`) | Services & Error Handling | [`rules/services-hooks.rules.md`](rules/services-hooks.rules.md) |
| Architecture, Directory Structure (`src/**`) | Vertical Slice Standards | [`rules/architecture.rules.md`](rules/architecture.rules.md) |
| Internationalization (`src/**`) | i18n & Localization Standards | [`rules/i18n.rules.md`](rules/i18n.rules.md) |
| Config & Package Setup (`*.json`, `package.json`) | Config & Dependency Setup | [`rules/config-setup.rules.md`](rules/config-setup.rules.md) |
| Unit / Integration Tests (`**/*.test.*`) | Testing Strategy & MSW | [`rules/testing.rules.md`](rules/testing.rules.md) |
| Commits, Git Flow | Conventional Commits Standard | [`rules/commits.rules.md`](rules/commits.rules.md) |

---

## 🧠 3. Autonomous Memory & Cogni Protocol
Autonomous memory queries, semantic signatures, and recall are delegated to [`skills/cogni/SKILL.md`](skills/cogni/SKILL.md).
