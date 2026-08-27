---
trigger: model_decision
description: 'Backend Workflow & Clean Standards for Node.js, Bun, Fastify and Express'
applyTo: 'src/**/*.ts, src/providers/**, src/modules/**'
---

# Rule: Backend Workflow & Clean Standards (Adely's Golden Standards)

> **ESTÁNDAR OBLIGATORIO DE DESARROLLO BACKEND:** Aplica a todo desarrollo backend en Node.js, Bun, Fastify y Express.

---

## 1. 📁 Estructura de Directorios (Public vs Private & Providers)

- **`src/providers/`**: Contiene únicamente proveedores de infraestructura y servicios transversales:
  - `logger.provider.ts` (Pino Logger estructurado).
  - `result.provider.ts` (Result Pattern + helper `sendResult`).
  - `appError.provider.ts` (Safety net para errorHandler global).
  - `database.provider.ts` (ORM / Query Builder).
  - `redis.provider.ts`, etc.
- **`src/modules/public/`**: Contiene los módulos y rutas que NO requieren autenticación previa (ej. `Auth/` con login, register, email validation).
- **`src/modules/private/`**: Contiene los módulos y rutas protegidos que requieren token JWT o sesión activa (ej. `Accounts/`, `Garage/`, `Routes/`, etc.).

---

## 2. ⚡ Result Pattern & Respuestas HTTP Unificadas

1. **Cero Excepciones en Servicios:**
   - Los servicios NUNCA lanzan excepciones (`throw`). Retornan siempre `Promise<Result<T>>`.
   - Utilizan `ResponseOk(data)` para éxito y `ResponseFail.X("ErrorCode")` para fallos (donde `X` representa el tipo de error HTTP: `BadRequest`, `Unauthorized`, `NotFound`, `UnprocessableEntity`, `Conflict`, `Internal`, etc.).
2. **Helper `sendResult` en Handlers/Controllers:**
   - Toda respuesta HTTP enviada por el servidor sigue estrictamente el mismo esquema JSON:
     - **Éxito (200 / 201):** `{ "message": "...", "data": T }`
     - **Fallo (4xx / 5xx):** `{ "error": "ErrorCode" }`

---

## 3. 🪵 Logs Estructurados (`src/providers/logger.provider.ts`)

- Usar exclusivamente Pino Logger para emitir logs de servidor (`logger.info`, `logger.warn`, `logger.error`).
- Loguear **únicamente lo estrictamente necesario** (inicio de servicios, fallos críticos con contexto, operaciones clave).
- Salida JSON estructurada en producción y formateada (pretty) en desarrollo.
- Queda estrictamente prohibido loguear contraseñas, tokens JWT completos o datos personales sensibles.

---

## 🧪 4. Colección de Pruebas Bruno (`.bru`) Obligatoria

- Por cada endpoint HTTP creado o modificado, se DEBE crear un archivo `.bru` en la carpeta `bruno/` correspondiente (`bruno/Public/` o `bruno/Private/`).
- El archivo `.bru` debe respetar la estructura completa de Bruno:
  - `meta`: nombre, secuencia y tipo (`http`).
  - `method` (get/post/put/delete): URL con variable `{{base_url}}`.
  - `headers`: Content-Type application/json.
  - `body:json`: payload de prueba.
  - `docs`: explicación del propósito del endpoint.
  - `example`: ejemplo de request y respuesta exitosa.

---

## 🛠️ 5. Regla de Oro de Utilidades (`src/utils/`)

- **Prohibición de Helpers Inline o Locales:** Queda estrictamente prohibido definir funciones auxiliares o helpers genéricos de validación, formateo o parsing (ej. `validateBody<T>()`, transformadores de fechas, validadores Zod, helpers criptográficos) dentro de un archivo de controlador (`*.controller.ts`) o servicio (`*.service.ts`).
- **Ubicación Obligatoria en `src/utils/`:** Cualquier función genérica susceptible de ser utilizada por más de un módulo o controlador DEBE crearse obligatoriamente dentro de `src/utils/` (ej: `validation.util.ts`, `crypto.util.ts`, `date.util.ts`) para garantizar DRY, modularidad y reutilización transversal.

---

## 🏥 6. Salud del Backend, Concurrencia, Caching & Pooling (Norma Universal)

> **REGLA DE SALUD INNEGOCIABLE:** Aplica independientemente del lenguaje o framework utilizado (TypeScript/Node/Bun, Go, Rust, Python, etc.).

1. **Ejecución Concurrente vs. Sequencial (Anti-Waterfall & Anti N+1):**
   - **Prohibido el efecto cascada (Waterfall):** Si en un mismo handler/servicio se ejecutan múltiples consultas I/O o a base de datos que son independientes entre sí, **JAMÁS** deben ejecutarse secuencialmente una detrás de otra (`await q1; await q2;`). Deben ejecutarse en paralelo (ej. `Promise.all([q1, q2])` o concurrencia nativa).
   - **Prohibido I/O dentro de bucles (Anti N+1):** Queda estrictamente prohibido realizar consultas a BD o llamadas API internas dentro de un bucle `for`, `map` o `forEach`. Deben agregarse mediante `IN (...)`, `JOIN` o batching.
2. **No Bloqueo de Hilo Principal (Non-Blocking Event Loop):**
   - Cada llamada HTTP debe ser asíncrona no bloqueante. Las operaciones de cálculo intensivo (CPU-bound) deben delegarse a hilos secundarios (Worker Threads) o colas asíncronas en segundo plano.
3. **Connection Pooling Obligatorio:**
   - Todo cliente o driver de base de datos relacional (PostgreSQL, MySQL, SQLite/Turso, etc.) **DEBE** configurarse con un Pool de Conexiones explícito (definir `max`, `idleTimeout`, `connectionTimeout`). Prohibido abrir/cerrar conexiones directas por cada petición HTTP.
4. **Caché Estratégico (Cache-Aside & TTL):**
   - Datos de alta frecuencia de lectura y baja frecuencia de actualización deben almacenarse en memoria (RAM / Redis) implementando el patrón *Cache-Aside* con un TTL (Time-To-Live) obligatorio.
5. **Límites de Concurrencia & Protecciones (Rate Limiting & Throttling):**
   - **Rate Limiting:** Toda API expuesta debe contar con limitador de velocidad de peticiones por cliente/IP (ej. Token Bucket / Redis Rate Limiter).
   - **Concurrency Throttling:** Si se ejecutan múltiples llamadas pesadas en paralelo hacia la BD o servicios de terceros, deben acotarse mediante un control/semáforo de concurrencia máximo para evitar agotamiento de sockets y memoria.
---

## 7. 🔌 Patrón Adaptador de Proveedores e Inversión de Dependencias (DIP)

> **REGLA DE ABSTRACCIÓN DE TERCEROS Y AGRUPACIÓN POR DOMINIO:** Ningún servicio de aplicación (`src/modules/**/*.service.ts`) debe depender o importar directamente tipos, nombres o funciones específicos de un proveedor externo o vendor concreto (ej. `calculateValhallaRoute`, `ValhallaTrip`, `StripeChargeDTO`, `TwilioSmsParams`).

1. **Estructura de Proveedores Agrupados por Dominio:**
   - Todos los proveedores de servicios se organizan en `src/providers/<domain>/` (ej: `src/providers/routing/`, `src/providers/payment/`, `src/providers/email/`).
   - La raíz del dominio expone la capa adaptadora agnóstica (`services/`, `interfaces/`, `index.ts`).
2. **Subdirectorio de Proveedores Concretos (`src/providers/<domain>/providers/<vendor>/`):**
   - Las implementaciones concretas de proveedores/vendors de un dominio residen en su subdirectorio `providers/<vendor>/` (ej: `src/providers/routing/providers/valhalla/`, `src/providers/payment/providers/stripe/`, `src/providers/email/providers/resend/`).
   - Esto evita la acumulación desordenada de carpetas en la raíz de `src/providers/`.
3. **Capa Adaptadora Agnóstica (Puerto / Adapter):**
   - La capa adaptadora en `src/providers/<domain>/services/` recibe las peticiones de los módulos de negocio, delega en la implementación del vendor activo (`src/providers/<domain>/providers/<vendor>/`) y traduce la respuesta hacia DTOs agnósticos.
4. **Reemplazo Transparente de Proveedores (Zero Breakage):**
   - Agregar o cambiar un proveedor (ej: agregar OSRM en `src/providers/routing/providers/osrm/`) **DEBE** ser transparente para la capa de negocio (`src/modules/`), requiriendo cero cambios en los servicios de aplicación.

---

## 8. 🗄️ Migraciones de Base de Datos Obligatorias y Autonomía sin MCP

> **REGLA DE ORO DE PERSISTENCIA Y MIGRACIONES:** Prohibido depender de ejecutores SQL de terceros o servidores MCP de base de datos para alterar esquemas en producción o desarrollo.

1. **Migraciones Físicas Obligatorias (`drizzle/*.sql` y `_journal.json`):**
   - Todo cambio o adición en el esquema ORM (`src/db/schema.ts`) **DEBE** generar/crear su correspondiente archivo de migración SQL dentro del directorio de migraciones (`drizzle/XXXX_nombre.sql`) y actualizar su índice en `drizzle/meta/_journal.json`.
2. **Autonomía Total del Servidor:**
   - El código backend debe ser 100% independiente. En el proceso de inicialización de la aplicación (`main.ts` / `runMigrations()`), el servidor ejecuta las migraciones automáticas sobre la base de datos de destino (`migrate(db, { migrationsFolder: "./drizzle" })`), garantizando el mismo esquema en cualquier entorno (Local, CI/CD, Staging, Producción).




