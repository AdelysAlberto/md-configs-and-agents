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
│   ├── react-native.md             # Límites de 250 LOC, DRY ScreenLayout y hooks
│   ├── engineering-invariants.md    # TypeScript funcional, Result Pattern, zero any, zero class
│   ├── cogni.md                    # Memoria semántica persistente y matching de tags
│   ├── commits.md                  # Convenciones Git y extracción de Ticket ID
│   ├── runtime.md                  # Política de ejecución, tool budget y circuit breakers
│   └── verification-checklist.md    # Checklist de comandos deterministas de verificación
├── agents/                         # Definiciones de agentes y subagentes (.json y .md)
│   ├── orchestrator.json           # Orquestador general (descompone y delega sin editar)
│   ├── profesor.json / .md         # El Profesor: Lead Architect & Project Orchestrator
│   ├── code-worker.json / .md      # Code Worker: Ejecutor técnico y refactorización
│   ├── sheldon.json / .md          # Sheldon Cooper: Arquitectura DDL, esquemas y contratos API
│   ├── edna.json / .md             # Edna Mode: Directora UX/UI, visual craft y wireframing
│   ├── tio-bob.json / .md          # Tio Bob: Reviewer senior de PR/MR y staged diffs (read-only)
│   ├── gorgory.json / .md          # Jefe Gorgory: Auditor de seguridad OWASP y dead code (read-only)
│   ├── saul.json / .md             # Saul Goodman: Asesor legal y regulatorio España/UE (read-only)
│   └── contador.json / .md         # Christian Wolff: Estratega fiscal y contable España/UE (read-only)
├── skills/                         # Librería modular de habilidades on-demand
│   ├── <skill-name>/
│   │   ├── skill.json              # Manifiesto declarativo para Pi
│   │   ├── instructions.md         # Instrucciones y patrones de implementación
│   │   └── SKILL.md                # Frontmatter canónico para auto-descubrimiento en OMP
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
- **Especialistas Ejecutores (`code-worker`, `edna`)**: Cuentan con herramientas de edición directa (`edit`, `write`, `bash`).
- **Especialistas de Auditoría y Dictamen (`sheldon`, `tio-bob`, `gorgory`, `saul`, `contador`)**: Operan en modo de sólo lectura (`read`, `grep`, `glob`, `bash` en modo seguro) para evitar modificaciones accidentales.

### 2.3 Doble Compatibilidad (`.json` + `.md`)
Para máxima interoperabilidad con cargadores declarativos y el CLI nativo de OMP (`omp`):
1. **Formato Declarativo JSON (`agents/*.json`)**:
   ```json
   {
     "name": "code-worker",
     "model": "deepseek/deepseek-chat",
     "rules": ["rules/react-native.md", "rules/engineering-invariants.md"],
     "skills": ["clean-architecture", "react-typescript-clean-code"],
     "tools": ["read", "edit", "write", "bash"]
   }
   ```
2. **Formato Nativo OMP Markdown (`agents/*.md`)**:
   ```markdown
   ---
   name: code-worker
   description: Specialist implementation and refactoring subagent.
   tools: read, edit, write, bash, grep, glob
   model: "@task"
   thinkingLevel: medium
   ---
   ```

### 2.4 Matriz de Roles

| Agente | Rol Principal | Modelo | Permisos / Tools |
| :--- | :--- | :--- | :--- |
| `orchestrator` / `profesor` | Estrategia, descomposición de tareas y delegación | `deepseek/deepseek-chat` / `@slow` | `read`, `grep`, `glob`, `task`, `ask`, `bash` |
| `code-worker` | Construcción de código, refactors y tests | `deepseek/deepseek-chat` / `@task` | `read`, `edit`, `write`, `bash`, `grep`, `glob` |
| `sheldon` | Arquitectura de sistemas, schemas DDL y APIs | `deepseek/deepseek-chat` / `@slow` | `read`, `grep`, `glob`, `write` (planes/specs) |
| `edna` | UX/UI, wireframes, tokens y visual craft | `deepseek/deepseek-chat` / `@slow` | `read`, `write`, `edit`, `grep`, `glob` |
| `tio-bob` | Revisión estricta de PR/MR y git diffs | `deepseek/deepseek-chat` / `@slow` | `read`, `grep`, `glob`, `bash` (Read-only) |
| `gorgory` | Auditoría de seguridad OWASP, rate limits y dead code | `deepseek/deepseek-chat` / `@slow` | `read`, `grep`, `glob`, `bash` (Read-only) |
| `saul` | Cumplimiento legal, RGPD, marcas e IP (España/UE) | `deepseek/deepseek-chat` / `@slow` | `read`, `grep`, `glob`, `bash` (Read-only) |
| `contador` | Contabilidad, fiscalidad, IRPF, RETA e IS (España/UE) | `deepseek/deepseek-chat` / `@slow` | `read`, `grep`, `glob`, `bash` (Read-only) |

---

## 3. Reglas y Stream Rules TTSR (`rules/`)

Las reglas imponen restricciones de calidad, estilo y arquitectura. En Oh My Pi funcionan como **Time-Traveling Stream Rules (TTSR)**: monitorean el stream de generación de herramientas en tiempo real y abortan la respuesta si se infringe una norma, **sin consumir tokens en el prompt base**.

### Ejemplo de Configuración TTSR (`rules/engineering-invariants.md`):
```yaml
---
description: Universal engineering invariants, pure functional TypeScript, zero any, zero class
globs:
  - "**/*.ts"
  - "**/*.tsx"
scope:
  - "tool:edit(*.ts)"
  - "tool:edit(*.tsx)"
  - "tool:write(*.ts)"
  - "tool:write(*.tsx)"
condition:
  - ":\\s*any\\b|as\\s+any\\b"
  - "\\bclass\\s+[A-Z]"
---
```

### Reglas Clave:
- **`react-native.md`**: Límite estricto de 250 LOC por archivo, pantallas `< 100 LOC`, uso obligatorio de `<ScreenLayout>` y prohibición de importar `SafeAreaView` desde `react-native`.
- **`engineering-invariants.md`**: TypeScript funcional (cero `class`, cero `this`), Result Pattern obligatorio (`{ success, data } | { success, error }`), vertical slicing en `src/modules/<Feature>/` y principio de inversión de dependencias (DIP).
- **`cogni.md`**: Consulta previa obligatoria (`cogni_search`) y persistencia determinista post-tarea (`cogni_save`).
- **`commits.md`**: Conventional Commits con extracción automática del ID de Ticket de la rama.
- **`runtime.md`**: Presupuesto de herramientas, control de contexto y circuit breakers de razonamiento.
- **`verification-checklist.md`**: Verificación determinista antes de completar cualquier entrega.

---

## 4. Estructura Híbrida de Skills (`skills/`)

Cada habilidad en `skills/<nombre>/` está empaquetada para ser consumida bajo demanda:

```text
skills/clean-architecture/
├── skill.json          # Declarativo: {"name": "...", "description": "...", "instructions": "instructions.md"}
├── instructions.md     # Contenido técnico detallado
└── SKILL.md            # Frontmatter YAML estándar para el descubridor nativo de OMP
```

### Catálogo de Skills Incluidos:
- **Arquitectura & Código**: `clean-architecture`, `backend-architecture`, `database-design`, `react-typescript-clean-code`, `react-native-architecture`, `css-architecture`, `zustand`, `astro`, `bun`, `cloudflare`.
- **Diseño & UX**: `visual-craft`, `ux-decision`, `mobile-native`, `frontend-design`, `ux-wireframing`, `screen-layout`, `impeccable`.
- **Calidad & Auditoría**: `testing-strategy`, `auditor`, `security-hardening`, `refactor-check`, `graphify`.
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

## 6. Flujo de Ejecución & Ahorro de Tokens

```text
[Usuario: Solicita nueva funcionalidad o refactorización]
                              │
                              ▼
                 [Agente: Profesor / Orchestrator]
             (Lee estructura, planifica y delega vía task)
                              │
        ┌─────────────────────┼─────────────────────┐
        ▼                     ▼                     ▼
[Subagente: Sheldon]   [Subagente: Edna]    [Subagente: Code-Worker]
(Ventana limpia)       (Ventana limpia)      (Ventana limpia)
(Skills: db, arch)     (Skills: visual, ux)  (Rules: react-native, invariants)
(Diseña schemas/APIs)  (Diseña wireframes)   (Implementa y testea)
        │                     │                     │
        └─────────────────────┼─────────────────────┘
                              ▼ (Devuelven resultado estructurado / diff)
                 [Agente: Profesor / Orchestrator]
                              │
                              ▼ (Verificación determinista final)
                    [Respuesta al Usuario]
```

---

## 7. Directrices Clave a Recordar

1. **Cero Context Bloat**: Nunca agregues manuales extensos al prompt principal. Conviértelos en un skill dentro de `skills/<nombre>/` e invócalos bajo demanda.
2. **Delegan los Orquestadores, Ejecutan los Workers**: El orquestador mantiene la visión holística; los workers ejecutan en contextos limpios.
3. **Verificación Determinista Obligatoria**: Antes de dar una tarea por finalizada, ejecuta:
   ```bash
   bun run biome:check && bun run check && bun test
   ```
4. **Respeto a las Stream Rules**: Cualquier código emitido con `: any`, `class` en TypeScript o layouts React Native duplicados será abortado automáticamente por TTSR.
