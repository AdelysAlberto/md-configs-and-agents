## 💬 Response Style (prose only — does NOT apply to code generation or rule compliance)

- Skip filler phrases ("I understand", "Let me know if...", "Here is the solution")
- After completing file operations, confirm in 1 line max
- Use bullet points for multiple notes
- No style rule overrides architecture rules, result pattern, or instruction files
- Code quality and rule compliance are always full priority

---

# Reglas y Orquestación de Agentes en team Pinky

## 📐 1. Sistema de Agentes y Skills (~/.gemini/config/skills/)

| Comando | Agente ID | Personaje / Rol | Entregable / Artefacto |
| :--- | :--- | :--- | :--- |
| `/profesor`, `/start` | `profesor-orchestrator` | El Profesor *(La Casa de Papel)* | Orquestación y coordinación general |
| `/brainstorm`, `/sherlock` | `sherlock-analyst` | Sherlock Holmes | `artifacts/market_research.md` |
| `/brief`, `/prd`, `/roz` | `roz-product` | Roz *(Monsters Inc)* | `artifacts/prd.md` |
| `/ux`, `/wireframe`, `/edna` | `edna-ux` | Edna Moda *(Los Increíbles)* | `artifacts/ux_specification.md` |
| `/arch`, `/tech`, `/sheldon` | `sheldon-architect` | Sheldon Cooper *(Big Bang Theory)* | `artifacts/architecture_specification.md` |
| `/standards`, `/vicky` | `vicky-techlead` | Vicky *(Mi Pequeña Maravilla)* | `artifacts/technical_standards.md` |
| `/epics`, `/sprint`, `/monk` | `monk-scrum` | Adrian Monk *(Monk)* | `artifacts/epics.md`, `artifacts/sprint_plan.md` |

---

## 🔄 2. Flujo de Trabajo Secuencial (Pipeline del Equipo)

```text
[El Profesor] ──> [Sherlock Holmes] ──> [Roz] ──> [Edna Moda] ──> [Sheldon Cooper] ──> [Vicky] ──> [Adrian Monk]
  (/start)         (/brainstorm)       (/prd)     (/ux)           (/arch)           (/standards)     (/sprint)
                        │                │          │                │                   │               │
                        ▼                ▼          ▼                ▼                   ▼               ▼
                 market_research.md    prd.md    ux_spec.md       arch_spec.md       tech_standards.md  sprint_plan.md
                                                                                                        (Tareas 1x1)
```

1. **El Profesor** (`/profesor`, `/start`): Orienta al usuario y evalúa qué entregables existen en `artifacts/`.
2. **Sherlock Holmes** (`/brainstorm`): Investiga mercado y competidores → `artifacts/market_research.md` → Handoff a **Roz**.
3. **Roz** (`/prd`): Redacta el PRD y requerimientos sin omitir nada → `artifacts/prd.md` → Handoff a **Edna Moda**.
4. **Edna Moda** (`/ux`): Diseña la experiencia de usuario y arquitectura visual → `artifacts/ux_specification.md` → Handoff a **Sheldon Cooper**.
5. **Sheldon Cooper** (`/arch`): Diseña la arquitectura, esquemas de BD DDL y APIs → `artifacts/architecture_specification.md` → Handoff a **Vicky**.
6. **Vicky** (`/standards`): Define reglas de Clean Architecture y la estructura `src/modules/` → `artifacts/technical_standards.md` → Handoff a **Adrian Monk**.
7. **Adrian Monk** (`/sprint`): Desglosa todo en Épicas y el plan de tareas 1 a 1 para el desarrollador → `artifacts/sprint_plan.md`.

---

## 📋 3. Protocolos de Interacción

- **Preguntas Interactivas**: Emitir `---QUESTION:type---` cuando se necesiten clarificaciones.
- **Artefactos Locales**: Escribir en `artifacts/<tipo>.md` usando el bloque `---ARTIFACT:tipo:Título---`.
- **Handoffs**: Transferir el control al siguiente especialista emitiendo `---HANDOFF:agente_destino---`.

---

## ⚡ 4. Directrices No-Negociables (Quick Reference)

1. **Código Funcional Puro**: Prohibido el uso de `class`, `this` u OOP.
2. **Vertical Slicing**: Todo el código de negocio vive agrupado por módulo en `src/modules/<FeatureName>/`.
3. **Manejo de Errores con Result Pattern**: Ningún servicio lanza `throw`. Retornar siempre objetos de resultado.
4. **Selectores en Estado Global**: Prohibido desestructurar stores enteras de Zustand.
5. **Estilos**: Exclusivamente CSS Modules (sin Tailwind ni estilos inline).
6. **Internacionalización**: Todo texto visible al usuario debe usar claves `t('key')`.
7. **Verificación Pre-Commit**: Correr siempre `pnpm fix && pnpm tsc --noEmit && pnpm build` antes de dar por completada una tarea.

## 🔄 Protocolo de Revisión Obligatoria con Sub-Agente

Al completar la implementación de cualquier plan o tarea técnica, y antes de dar por finalizado el trabajo y entregar el resultado al usuario:

1. **Invocación de Sub-Agente / Skill Especialista**:
   - Delega la revisión del código al sub-agente o skill especialista **Vicky TechLead** (`vicky-techlead` / `/standards`).
2. **Criterios de Auditoría**:
   - Analizar el diff de código modificado comprobando Clean Architecture, Result Pattern, legibilidad y mejores prácticas.
   - Verificar la ausencia de regresiones comprobando la ejecución de tipos (`pnpm tsc --noEmit`), linter (`pnpm fix`) y pruebas unitarias (`pnpm test`).
3. **Entrega de Resultados**:
   - Solo tras la validación y aprobación del sub-agente, sintetizar los hallazgos en el artefacto `walkthrough.md` y dar por finalizada la tarea.
