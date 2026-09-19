# Team Pinky - Arquitectura de Agentes, Skills y Reglas para Pi & Oh My Pi (OMP)

Ecosistema modular de ingeniería, diseño, auditoría y orquestación técnica adaptado a la arquitectura de **Pi** y **Oh My Pi (OMP)**. Se fundamenta en tres pilares: **contextos efímeros**, **tipado estricto** e **inyección declarativa on-demand**.

A diferencia de los entornos que sobrecargan el prompt inicial con cientos de reglas y manuales estáticos, este sistema inyecta conocimiento específico únicamente al agente y tarea que lo requiere, interceptando errores en tiempo real mediante **Time-Traveling Stream Rules (TTSR)**.

---

## 1. Estructura de Directorios

```text
agents-pi/
├── config.json                     # Modelos, providers, keys, perfiles y runtime TTSR
├── mcp.json                        # Configuración de servidores Model Context Protocol
├── AGENTS.md                       # Registro principal, mapa de delegación e invariantes globales
├── rules/                          # Reglas globales y reactivas (Stream Rules / TTSR)
│   ├── frontend.md                 # React 19+, Zustand, TanStack Query, <250 LOC, base components, no inline styles
│   ├── backend.md                  # Bun, Fastify/Express, Service-Repository, Bruno collections (.bru), Pino
│   ├── react-native.md             # Límites de 250 LOC, DRY ScreenLayout, hooks y árbol de decisión Modal vs Pantalla
│   ├── engineering-invariants.md   # TypeScript funcional, Result Pattern, Screaming Architecture, pure utils, SSOT
│   ├── cogni.md                    # Memoria semántica persistente y matching de tags
│   ├── commits.md                  # Convenciones Git y extracción de Ticket ID
│   ├── runtime.md                  # Política de ejecución, tool budget y circuit breakers
│   └── verification-checklist.md   # Checklist de comandos deterministas de verificación (Biome, Bruno, no deprecations)
├── agents/                         # Definiciones de agentes y subagentes (.json y .md)
│   ├── orchestrator.json           # Orquestador general (descompone y delega sin editar)
│   ├── profesor.json / .md         # El Profesor: Lead Architect & Project Orchestrator
│   ├── homero.json / .md           # Homero Simpson: Senior Polyglot Worker (Frontend, Backend, Mobile, Go, Rust, Python, Infra)
│   ├── code-worker.json / .md      # Code Worker: Ejecutor técnico y refactorización
│   ├── sheldon.json / .md          # Sheldon Cooper: Full-Stack Architect (Backend, DB, Frontend, Mobile & APIs)
│   ├── edna.json / .md             # Edna Mode: Directora UX/UI, visual craft y wireframing (Estricto UI/UX)
│   ├── tio-bob.json / .md          # Tio Bob: Reviewer senior de PR/MR y staged diffs (read-only)
│   ├── gorgory.json / .md          # Jefe Gorgory: Auditor de seguridad OWASP y dead code (read-only)
│   ├── saul.json / .md             # Saul Goodman: Asesor legal y regulatorio España/UE (read-only)
│   └── contador.json / .md         # Christian Wolff: Estratega fiscal y contable España/UE (read-only)
├── skills/                         # Librería modular de habilidades on-demand
│   ├── <skill-name>/
│   │   ├── skill.json              # Manifiesto declarativo para Pi
│   │   ├── instructions.md         # Instrucciones y patrones de implementación
│   │   └── SKILL.md                # Frontmatter canónico para auto-descubrimiento en OMP
│   └── plan/                       # Sistema iterativo de planes técnicos estructurados (<TOPIC>_PLAN.md)
├── commands/                       # Slash commands ejecutables
│   └── linkedin.md                 # Generador de reflexiones de ingeniería técnica
└── prompts/                        # Prompts modulares reutilizables
    └── creative.md                 # Directivas de alta creatividad para Edna Mode
```

---

## 2. Agentes y Subagentes (`agents/`)

### 2.1 Aislamiento de Contexto Efímero
Los subagentes **no heredan el historial de chat acumulado** del agente principal ni de turnos previos. Se inicializan en una **ventana de contexto limpia** con sus reglas y skills asignadas, ejecutan su tarea y devuelven un resultado estructurado o diff al orquestador.

### 2.2 Principio de Mínimo Privilegio en Herramientas
- **Orquestadores (`orchestrator`, `profesor`)**: Disponen de herramientas de lectura, inspección y delegación (`read`, `grep`, `glob`, `task`, `ask`). Tienen prohibido mutar código directamente en flujos de orquestación.
- **Especialistas Ejecutores (`homero`, `code-worker`)**: Cuentan con herramientas de edición directa (`edit`, `write`, `bash`). Homero es el obrero senior políglota con criterio técnico, Clean Code y cero antipatrones.
- **Especialista de Diseño (`edna`)**: Enfocada exclusivamente en visual craft, wireframes, tokens CSS y componentes UI/UX. No implementa lógica backend.
- **Especialistas de Auditoría y Dictamen (`sheldon`, `tio-bob`, `gorgory`, `saul`, `contador`)**: Operan en modo de sólo lectura (`read`, `grep`, `glob`, `bash` en modo seguro) para evitar modificaciones accidentales.

### 2.3 Doble Compatibilidad (`.json` + `.md`)
Para máxima interoperabilidad con cargadores declarativos y el CLI nativo de OMP (`omp`):
1. **Formato Declarativo JSON (`agents/*.json`)**:
   ```json
   {
     "name": "homero",
     "model": "deepseek/deepseek-chat",
     "rules": ["rules/engineering-invariants.md", "rules/frontend.md", "rules/backend.md"],
     "skills": ["react-typescript-clean-code", "zustand", "testing-strategy"],
     "tools": ["read", "edit", "write", "bash", "grep", "glob"]
   }
   ```
2. **Formato Nativo OMP Markdown (`agents/*.md`)**:
   ```markdown
   ---
   name: homero
   description: Senior polyglot software worker and pragmatic craftsman.
   tools: read, edit, write, bash, grep, glob
   model: "@task"
   thinkingLevel: medium
   ---
   ```

### 2.4 Matriz de Roles

| Agente | Rol Principal | Modelo | Permisos / Tools |
| :--- | :--- | :--- | :--- |
| `orchestrator` / `profesor` | El Maestro Mayor: Estrategia, descomposición y delegación | `deepseek/deepseek-chat` / `@slow` | `read`, `grep`, `glob`, `task`, `ask`, `bash` |
| `homero` | El Obrero Senior: Construcción políglota (Frontend, Backend, Mobile, Go, Rust, Python, Infra) | `deepseek/deepseek-chat` / `@task` | `read`, `edit`, `write`, `bash`, `grep`, `glob` |
| `code-worker` | Ejecutor técnico puntual y refactorizaciones específicas | `deepseek/deepseek-chat` / `@task` | `read`, `edit`, `write`, `bash`, `grep`, `glob` |
| `sheldon` | El Arquitecto: Full-Stack Architecture, planes técnicos, schemas DDL y APIs | `deepseek/deepseek-chat` / `@slow` | `read`, `grep`, `glob`, `write` (planes/specs) |
| `edna` | Diseñadora UX/UI: Visual craft, layout wireframes, tokens y accesibilidad | `deepseek/deepseek-chat` / `@slow` | `read`, `write`, `edit`, `grep`, `glob` |
| `tio-bob` | El Inspector: Revisión estricta de PR/MR y git diffs | `deepseek/deepseek-chat` / `@slow` | `read`, `grep`, `glob`, `bash` (Read-only) |
| `gorgory` | Seguridad: Auditoría OWASP, rate limits, secrets y dependencias | `deepseek/deepseek-chat` / `@slow` | `read`, `grep`, `glob`, `bash` (Read-only) |
| `saul` | Jurídico: Cumplimiento legal, RGPD, marcas e IP (España/UE) | `deepseek/deepseek-chat` / `@slow` | `read`, `grep`, `glob`, `bash` (Read-only) |
| `contador` | Fiscal: Contabilidad, IRPF, RETA, IS y optimización tributaria (España/UE) | `deepseek/deepseek-chat` / `@slow` | `read`, `grep`, `glob`, `bash` (Read-only) |

---

## 3. Reglas y Stream Rules TTSR (`rules/`)

Las reglas imponen restricciones de calidad, estilo y arquitectura. En Oh My Pi funcionan como **Time-Traveling Stream Rules (TTSR)**: monitorean el stream de generación de herramientas en tiempo real y abortan la respuesta si se infringe una norma, **sin consumir tokens en el prompt base**.

### Reglas Clave:
- **`frontend.md`**: React 19+, Zustand (selectores atómicos), TanStack Query (custom query hooks con estados de Loading aislados), límite de 250 LOC, componentes base reutilizables, prohibición de estilos inline, funciones utilitarias puras en `utils/`.
- **`backend.md`**: Runtime Bun por defecto, Fastify o Express con separación limpia (Controller -> Service -> Repository), Result Pattern obligatorio, colecciones Bruno (`.bru`) para cada endpoint, logging estructurado con Pino.
- **`react-native.md`**: Límite de 250 LOC por archivo, pantallas `< 100 LOC`, uso de `<ScreenLayout>`, árbol de decisión Modal vs Pantalla (cero apilamiento de modales).
- **`engineering-invariants.md`**: TypeScript funcional (cero `class`, cero `this`), cero APIs deprecadas, Screaming Architecture, Single Source of Truth (SSOT), principios políglotas (Go, Rust, Python, Java).
- **`cogni.md`**: Consulta previa obligatoria (`cogni_search`) y persistencia determinista post-tarea (`cogni_save`).
- **`commits.md`**: Conventional Commits con extracción automática del ID de Ticket de la rama.
- **`runtime.md`**: Presupuesto de herramientas, control de contexto y circuit breakers de razonamiento.
- **`verification-checklist.md`**: Verificación determinista antes de completar cualquier entrega (Biome, TypeScript, Bruno collections, deprecation audit).

---

## 4. Estructura Híbrida de Skills (`skills/`)

Cada habilidad en `skills/<nombre>/` es una guía de procedimiento ejecutable bajo demanda:

```text
skills/plan/
├── skill.json          # Declarativo: {"name": "plan", "description": "...", "instructions": "instructions.md"}
├── instructions.md     # Fases de planificación interactiva, loop de preguntas y plantilla de PLAN.md
└── SKILL.md            # Frontmatter YAML estándar para el descubridor nativo de OMP
```

### Catálogo de Skills On-Demand:
- **Planificación**: `plan` (elaboración de `<TOPIC>_PLAN.md` interactivo con status `PENDING`, análisis técnico y buenas prácticas).
- **Arquitectura & Código**: `backend-architecture`, `database-design`, `react-typescript-clean-code`, `react-native-architecture`, `css-architecture`, `zustand`.
- **Diseño & UX**: `visual-craft`, `ux-decision`, `mobile-native`, `frontend-design`, `ux-wireframing`, `impeccable`.
- **Calidad & Auditoría**: `testing-strategy`, `auditor`, `security-hardening`, `graphify`.
- **Producto & Crecimiento**: `product-requirements`, `scrum-planning`, `market-research`, `growth-copywriting`, `i18n-localization`.
- **Especialistas Legales, Fiscales & Comunicación**: `legal-compliance`, `tax-accounting`, `finch`, `linkedin`, `cogni`.

---

## 5. Integración Model Context Protocol (`mcp.json`)

Conexión a servidores MCP locales y remotos para ampliar capacidades:

```json
{
  "mcpServers": {
    "cogni": {
      "command": "/Users/adelysalberto/.local/bin/cogni",
      "args": ["mcp"]
    },
    "viasera-notifier": {
      "command": "bun",
      "args": ["run", "/Volumes/Datos/Projects/viasera/mcp/n8n-notifier/src/index.ts"],
      "env": {
        "N8N_WEBHOOK_URL": "http://<N8N_HOST>:5678/webhook/viasera-agent-notify"
      }
    },
    "postgres": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-postgres",
        "postgresql://viasera_admin:viasera_secret_password_2026@127.0.0.1:5432/viasera_db"
      ]
    },
    "pencil": {
      "command": "/Applications/Pencil.app/Contents/Resources/app.asar.unpacked/out/mcp-server-darwin-arm64",
      "args": ["--app", "desktop"]
    }
  }
}
```

---

## 6. Flujo de Ejecución Metáfora de Construcción

```text
[Usuario: Solicita nueva funcionalidad o refactorización]
                              │
                              ▼
                  [El Maestro: El Profesor]
         (Analiza requerimiento, inicializa contexto limpio)
                              │
                              ▼
                 [El Arquitecto: Sheldon]
     (skills/plan -> formula preguntas -> redacta <TOPIC>_PLAN.md)
                              │
                              ▼
                     [Aprobación del Usuario]
                              │
        ┌─────────────────────┼─────────────────────┐
        ▼                                           ▼
[Diseño UX/UI: Edna]                    [El Obrero Senior: Homero]
(Visual craft, wireframes,              (Construcción políglota Frontend/Backend,
tokens, componentes base)               Clean Code, tests, colecciones Bruno)
        │                                           │
        └─────────────────────┬─────────────────────┘
                              ▼
                  [El Inspector: Tio Bob]
            (Revisión de diffs, Biome, tests, sin antipatrones)
                              │
                              ▼
                  [El Maestro: El Profesor]
                 (Verificación determinista final)
                              │
                              ▼
                    [Respuesta al Usuario]
```

---

## 7. Directrices Clave a Recordar

1. **Cero Context Bloat**: Las normas de stack y patrones van en `rules/` (se leen como reglas invariantes o TTSR). Los manuales procedimentales van en `skills/<nombre>/` y se invocan bajo demanda.
2. **Delegan los Orquestadores, Ejecutan los Workers**: El Profesor mantiene la visión holística; Homero y los obreros ejecutan en contextos limpios con maestría técnica.
3. **Verificación Determinista Obligatoria**: Antes de dar una tarea por finalizada, ejecuta:
   ```bash
   bun run biome:check && bun run check && bun test
   ```
4. **Respeto a las Invariantes de Stack**: Prohibido el uso de `: any`, `class` en TypeScript, estilos inline, endpoints sin colección Bruno o vistas que excedan las 250 líneas.
