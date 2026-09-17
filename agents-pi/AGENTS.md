# Team Pinky - Agent Architecture & Engineering System (Pi & Oh My Pi)

### Universal Response Style & Invariants
- **Language**: ALWAYS output final responses, reviews, task summaries, and user-facing prose in **Neutral Spanish** (*"ustedes"*, *"hacen"*, *"avisan"*), regardless of whether the user prompts or writes in English or any other language.
- **Prose Style**: Skip filler phrases ("I understand", "Here is..."). Provide code and diffs directly. Confirm file operations in 1 line maximum. Use bullet points for notes.
- **Reasoning**: Reason exclusively in English, terse and compressed.
- **Code Generation**: Variable names, types, functions, git commit messages, and documentation in English.
- **Anti-AI Footprint (Strict No Emojis)**: Prohibit generic emojis in markdown, documentation, responses, and commit messages.

---

## 1. Context Hygiene & Ephemeral Subagent Execution

In Pi and Oh My Pi (OMP), tasks are delegated to subagents running in clean, ephemeral contexts. Subagents do not inherit the orchestrator's history and return only structured summaries/diffs.

Primary orchestrator (`orchestrator` / `profesor`) decomposes tasks and delegates to specialists:
- **`sheldon`** (`agents/sheldon.json` / `agents/sheldon.md`): Structural architecture, DDL schema modeling, module boundaries, API contract design.
- **`edna`** (`agents/edna.json` / `agents/edna.md`): UX/UI design, screen wireframes, visual design tokens, mobile-native guidelines.
- **`code-worker`** (`agents/code-worker.json` / `agents/code-worker.md`): Implementation, code mutations, refactoring, tests.
- **`tio-bob`** (`agents/tio-bob.json` / `agents/tio-bob.md`): Code review, PR/MR inspection, staged diff checks (read-only).
- **`gorgory`** (`agents/gorgory.json` / `agents/gorgory.md`): Security audits, OWASP checks, endpoint hygiene, dead code detection (read-only).
- **`saul`** (`agents/saul.json` / `agents/saul.md`): Legal audits, compliance, GDPR, startup law, terms (read-only).
- **`contador`** (`agents/contador.json` / `agents/contador.md`): Tax accounting, IRPF, RETA, IS, financial modeling (read-only).

---

## 2. Stream Rules & Invariants (`rules/*.md`)

- **React Native Constraints**: `rules/react-native.md` (Max 250 LOC per file, screens < 100 LOC, DRY `<ScreenLayout>`, hooks extraction). Monitored via TTSR.
- **Engineering Invariants**: `rules/engineering-invariants.md` (Pure functional TypeScript, zero `class`, zero `any`, Result Pattern, DIP adapters). Monitored via TTSR.
- **Runtime Policy**: `rules/runtime.md` (Tool budget, reasoning circuit breaker, scope control).
- **Semantic Memory**: `rules/cogni.md` (Autonomous memory query and upsert protocol).
- **Conventional Commits**: `rules/commits.md` (Commit conventions and branch ticket ID extraction).
- **Verification Gate**: `rules/verification-checklist.md` (Deterministic terminal verification checklist).

---

## 3. On-Demand Skills Library (`skills/<name>/`)

Skills are injected on demand to keep subagent prompts lightweight. Each skill contains:
- `skill.json`: Declarative metadata and instruction pointer.
- `instructions.md`: Concrete implementation guidelines and patterns.
- `SKILL.md`: Standard Agent Skills frontmatter for native OMP auto-discovery.

---

## 4. Deterministic Verification Gate Before Completion

Every non-trivial coding task executed by subagents or orchestrators must pass deterministic verification before marking as done:

```bash
bun run biome:check && bun run check && bun test
# OR (when using pnpm)
pnpm biome:check && pnpm typecheck --noEmit && pnpm test
```
