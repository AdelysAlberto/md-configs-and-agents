Necesito que modularices mi archivo `.github/copilot-instructions.md` porque es demasiado grande y estoy perdiendo contexto/reglas cuando trabajo.

## Objetivo

Dividir el archivo monolítico en archivos `.instructions.md` modulares dentro de `.github/instructions/`, usando el sistema de YAML frontmatter `applyTo` de VS Code Copilot para que se carguen automáticamente según el archivo que esté editando.

## Pasos a seguir

1. **Analizar** el contenido actual de `.github/copilot-instructions.md` y cualquier otro archivo `.md` en `.github/` (buscar contradicciones entre archivos)

2. **Corregir contradicciones** entre archivos antes de modularizar

3. **Crear archivos modulares** en `.github/instructions/` con estos nombres y convenciones:
   - Nombre: `{dominio}.instructions.md` (kebab-case, siempre termina en `.instructions.md`)
   - Cada archivo DEBE tener YAML frontmatter con `applyTo` que define los glob patterns de activación
   - Formato del frontmatter:
     ```yaml
     ---
     applyTo: "glob/pattern/**, otro/pattern/**"
     ---
     ```

4. **Archivos estándar a crear** (adaptar según el proyecto):

   | Archivo | applyTo | Contenido |
   |---|---|---|
   | `styling.instructions.md` | Componentes, archivos de estilos, constantes de colores/fuentes | Sistema de estilos, colores, theming, responsive, fuentes |
   | `architecture.instructions.md` | Directorios principales de código fuente | Estructura del proyecto, patrones arquitectónicos, layouts, navegación |
   | `components.instructions.md` | Archivos de componentes UI | Componentes base/reutilizables, principios DRY, diseño de componentes |
   | `services-hooks.instructions.md` | Services, hooks, HTTP client | Patrón de servicios, HTTP client, hooks de data fetching |
   | `i18n.instructions.md` | Todos los archivos de código + archivos de traducción | Reglas de internacionalización, patrones, keys dinámicas |
   | `state-management.instructions.md` | Stores, hooks | Estado global, server state, persistencia |
   | `coding-standards.instructions.md` | Todos los archivos de código | TypeScript, paradigma de programación, seguridad, accesibilidad |
   | `ux-design.instructions.md` | Archivos de pantallas/páginas | UX best practices, estándares UI |
   | `config-setup.instructions.md` | Archivos de configuración | Versiones del stack, scripts, linter, tsconfig |
   | `verification-checklist.instructions.md` | Todos los archivos (`**`) | Checklist de verificación post-tarea |

   Si el proyecto tiene dominios adicionales (testing, CI/CD, database, auth, etc.), crear archivos adicionales.

5. **Reescribir `copilot-instructions.md`** como un archivo lean (~100-150 líneas) que contenga SOLO:
   - Identidad del proyecto (1-2 líneas)
   - Stack tecnológico (versiones actuales)
   - Reglas críticas globales (las 10-12 más importantes, 2-3 líneas cada una, con referencia → al archivo modular)
   - Tabla de archivos modulares con rutas completas `.github/instructions/nombre.instructions.md`
   - Tabla de archivos de referencia adicionales en `.github/`
   - Tabla de utilidades/lookup rápido del proyecto
   - Comandos de verificación

6. **Hacer backup** del archivo original como `.github/copilot-instructions-backup.md`

## Reglas de calidad para los archivos modulares

- Cada archivo debe ser **autocontenido** — tiene todo lo necesario para ese dominio
- NO repetir contenido extenso entre archivos — si dos archivos necesitan la misma info, uno referencia al otro
- Incluir **ejemplos de código** concretos de ✅ correcto y ❌ incorrecto
- Máximo ~150 líneas por archivo modular
- Los `applyTo` deben ser glob patterns válidos separados por coma

## Resultado esperado

- `copilot-instructions.md`: ~100-150 líneas (índice + reglas críticas)
- `.github/instructions/`: 8-12 archivos modulares de ~50-150 líneas cada uno
- Cero contradicciones entre archivos
- Backup del original

## Verificación final

Después de terminar, muéstrame:
1. Conteo de líneas de todos los archivos (wc -l)
2. Lista de contradicciones encontradas y corregidas
3. Tabla resumen de antes vs después
