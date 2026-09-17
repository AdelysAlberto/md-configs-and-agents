# Team Pinky - Agent Architecture & Engineering System (OpenCode)

### Universal Response Style & Invariants
- **Language**: ALWAYS output final responses, reviews, task summaries, and user-facing prose in **Neutral Spanish** (*"ustedes"*, *"hacen"*, *"avisan"*), regardless of whether the user prompts or writes in English or any other language.
- **Prose Style**: Skip filler phrases ("I understand", "Here is..."). Provide code and diffs directly. Confirm file operations in 1 line maximum. Use bullet points for notes.
- **Reasoning**: Reason exclusively in English, terse and compressed.
- **Code Generation**: Variable names, types, functions, git commit messages, and documentation in English.
- **Anti-AI Footprint (Strict No Emojis)**: Prohibit generic emojis in markdown, documentation, responses, and commit messages.

## 1. External File Loading & Lazy Rules

CRITICAL: When you encounter a file reference (e.g., `@rules/engineering-invariants.rules.md` or `@skills/<name>/SKILL.md`), use your Read tool to load it on a need-to-know basis. They are relevant to the SPECIFIC task at hand.

Instructions:
- Do NOT preemptively load all references: use lazy loading based on actual need.
- When loaded, treat content as mandatory instructions that override defaults.
- Follow references recursively when needed.

## Architectural Exploration and Knowledge Graph (Graphify)
- **Graph Source**: When asked about global architecture, dependency analysis between modules, detection of god nodes or flows between layers, the agent MUST prioritize the inspection of `graphify-out/` and execute the `graphify` skill.
- **Impact Preflight**: Before cross-sectional refactoring or service deletions, consult the knowledge graph to map non-obvious couplings and dependencies.

### Core Universal Rules
- For reasoning budget, tool limits, and execution policy: `@rules/runtime.rules.md`
- For TypeScript standards, Result Pattern, and code invariants: `@rules/engineering-invariants.rules.md`
- For semantic memory retrieval and persistence: `@rules/cogni.rules.md`
- For Conventional Commits and branch Ticket ID extraction: `@rules/commits.rules.md`
- For deterministic verification before completing tasks: `@rules/verification-checklist.rules.md`

### Notificaciones de Intervención Humana (El Pregonero)
Antes de ejecutar `ask_question`, pausar para aprobación de un plan, solicitar permisos de ejecución o concluir una tarea principal, DEBES invocar la herramienta MCP `send_telegram_alert` (`el-pregonero`) con los parámetros:
- `event_type`: 'QUESTION' | 'PLAN_APPROVAL' | 'PERMISSION_REQUIRED' | 'TASK_COMPLETED' | 'ERROR'
- `summary`: Resumen conciso de la situación o pregunta.
- `action_required`: Acción o respuesta requerida por el usuario.


---

## 2. Subagent Delegation Policy

Primary orchestrators (`@profesor`) must delegate specialized tasks to subagents via the `task` tool to preserve context hygiene:
- **`@sheldon`** (`agents/sheldon.md`): Delegate when structural architecture, DDL schema modeling, module boundaries, API contract design, or AST indexing maps are required.
- **`@edna`** (`agents/edna.md`): Delegate when UX/UI design, screen wireframes, design tokens, or visual copy is required.
- **`@tio-bob`** (`agents/tio-bob.md`): Delegate when the user asks for code review, PR/MR inspection, or staged diff checks.
- **`@gorgory`** (`agents/gorgory.md`): Delegate when the user asks for security audits, OWASP checks, endpoint hygiene, repository health, or dead code detection.
- **`@saul`** (`agents/saul.md`): Delegate when the user asks for legal audits, terms & conditions review, startup/corporate incorporation (Spain/EU), tax/pluriactivity compliance, trademark registration, software copyright (LPI), or GDPR/privacy analysis.
- **`@contador`** (`agents/contador.md`): Delegate when the user asks for financial calculations, IRPF brackets, RETA quotas, corporate tax (IS), tax deductions, accounting optimization, or tax models (130, 303, 111, 115, 200, 349, 369).

---

## 3. On-Demand Skills Library (`skills/<name>/SKILL.md`)

Agents do not carry heavy technical manuals in their base prompt. Instead, they dynamically inspect and read specialized skills via the `skill` tool when addressing specific domains:

| Category | Skill Reference | Domain Knowledge |
| :--- | :--- | :--- |
| **Tax & Accounting** | `@skills/tax-accounting/SKILL.md` | Spanish & EU tax, IRPF brackets, RETA tiers, Corporate Tax (IS), legal deductions, VAT/OSS. |
| **Legal & Compliance** | `@skills/legal-compliance/SKILL.md` | Spanish & EU law, Ley de Startups 28/2022, pluriactivity/RETA, S.L. Crea y Crece, IP/LPI, trademarks, GDPR/ePrivacy, PSD2, AI Act. |
| **Architecture** | `@skills/clean-architecture/SKILL.md` | Vertical Slicing (`src/modules/`), Result Pattern, Pure Functional TS, DIP adapters. |
| **Architecture** | `@skills/backend-architecture/SKILL.md` | Fastify / Express / Bun, public/private route isolation, structured Pino logs, Bruno tests. |
| **Database** | `@skills/database-design/SKILL.md` | PostgreSQL, Drizzle ORM, physical migrations, indexing (B-Tree, GIN), Redis caching, ACID. |
| **Styling** | `@skills/css-architecture/SKILL.md` | CSS Modules (`*.module.css`), strict BEM naming, Design Tokens (CSS vars), GPU animations. |
| **UI Design** | `@skills/ux-wireframing/SKILL.md` | Screen anatomy wireframes, dramatic minimalism ("No capes!"), user journeys, state transitions. |
| **UI Design** | `@skills/frontend-design/SKILL.md` | Visual direction, typography, distinct human aesthetics, avoiding templated AI clichés. |
| **State** | `@skills/zustand/SKILL.md` | Zustand 5+, atomic selectors (`useShallow`), slice segregation, avoiding infinite render loops. |
| **Frontend** | `@skills/react-typescript-clean-code/SKILL.md` | React 18/19+, hook hygiene (`useEffect` vs derivations), strict typing without `any`. |
| **Mobile** | `@skills/react-native-architecture/SKILL.md` | React Native & Expo, cross-platform navigation, offline synchronization. |
| **Testing** | `@skills/testing-strategy/SKILL.md` | Vitest, React Testing Library, Mock Service Worker (MSW), service Result Pattern testing. |
| **Planning** | `@skills/scrum-planning/SKILL.md` | Epics, User Stories, Gherkin acceptance criteria, granular 1x1 developer tasks. |
| **Product** | `@skills/product-requirements/SKILL.md` | Product Briefs, PRDs, MoSCoW prioritization, functional & non-functional requirements. |
| **Discovery** | `@skills/market-research/SKILL.md` | Deductive competitor analysis, feature parity matrices, user pain point validation. |
| **Growth** | `@skills/growth-copywriting/SKILL.md` | High-conversion copy, sales persuasion frameworks (AIDA, PAS), landing blueprints. |
| **Security** | `@skills/security-hardening/SKILL.md` | OWASP Top 10 defenses, endpoint rate limiting, secure cookie flags, token handling. |
| **Audit** | `@skills/auditor/SKILL.md` | Static codebase discovery, architecture mapping, technical debt evaluation. |
| **i18n** | `@skills/i18n-localization/SKILL.md` | react-i18next namespaces, translation key hygiene, pluralization, RTL logical properties. |
| **Runtime & Ops**| `@skills/bun/SKILL.md` | Bun runtime APIs, test runner, bundler, Bun.serve, shell scripts. |
| **Runtime & Ops**| `@skills/cloudflare/SKILL.md` | Workers, Pages, KV, D1, R2, Vectorize. |
| **Memory** | `@skills/cogni/SKILL.md` | Autonomous memory system for semantic signatures in local/global SQLite. |
| **Writing** | `@skills/finch/SKILL.md` | Natural human tone technical writing for documentation and proposals. |
| **Social / Tech** | `@skills/linkedin/SKILL.md` | Authentic engineering reflections (Finch + Edna style, no emojis, no clichés). |

---

## 4. Verification Gate Before Completion

Every non-trivial coding task executed by `@profesor` must pass deterministic verification before marking as done:

```bash
bun run biome:check && bun run check && bun test
# OR (when using pnpm)
pnpm biome:check && pnpm typecheck --noEmit && pnpm test
```
