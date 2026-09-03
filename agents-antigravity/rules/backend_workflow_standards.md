---
trigger: model_decision
description: 'Backend Workflow & Clean Standards for Node.js, Bun, Fastify and Express'
applyTo: 'src/**/*.ts, src/providers/**, src/modules/**'
---

# Rule: Backend Workflow & Clean Standards (Adely's Golden Standards)

> **MANDATORY BACKEND DEVELOPMENT STANDARD:** Applies to all backend development in Node.js, Bun, Fastify, and Express.

---

## 1. Directory Structure (Public vs Private & Providers)

- **`src/providers/`**: Contains only infrastructure providers and cross-cutting services:
  - `logger.provider.ts` (Structured Pino Logger).
  - `result.provider.ts` (Result Pattern + `sendResult` helper).
  - `appError.provider.ts` (Safety net for global errorHandler).
  - `database.provider.ts` (ORM / Query Builder).
  - `redis.provider.ts`, etc.
- **`src/modules/public/`**: Contains modules and routes that do NOT require prior authentication (e.g., `Auth/` with login, register, email validation).
- **`src/modules/private/`**: Contains protected modules and routes that require JWT token or active session (e.g., `Accounts/`, `Garage/`, `Routes/`, etc.).

---

## 2. Result Pattern & Unified HTTP Responses

1. **Zero Exceptions in Services:**
   - Services NEVER throw exceptions (`throw`). They always return `Promise<Result<T>>`.
   - Use `ResponseOk(data)` for success and `ResponseFail.X("ErrorCode")` for failures (where `X` represents the HTTP error type: `BadRequest`, `Unauthorized`, `NotFound`, `UnprocessableEntity`, `Conflict`, `Internal`, etc.).
2. **`sendResult` Helper in Handlers/Controllers:**
   - Every HTTP response sent by the server strictly follows the same JSON schema:
     - **Success (200 / 201):** `{ "message": "...", "data": T }`
     - **Failure (4xx / 5xx):** `{ "error": "ErrorCode" }`

---

## 3. Structured Logs (`src/providers/logger.provider.ts`)

- Use exclusively Pino Logger for emitting server logs (`logger.info`, `logger.warn`, `logger.error`).
- Log **only what is strictly necessary** (service startup, critical failures with context, key operations).
- Structured JSON output in production and formatted (pretty) output in development.
- It is strictly prohibited to log passwords, full JWT tokens, or sensitive personal data.

---

## 4. Mandatory Bruno Test Collection (`.bru`)

- For every HTTP endpoint created or modified, a `.bru` file MUST be created in the corresponding `bruno/` folder (`bruno/Public/` or `bruno/Private/`).
- The `.bru` file must follow the complete Bruno structure:
  - `meta`: name, sequence, and type (`http`).
  - `method` (get/post/put/delete): URL with `{{base_url}}` variable.
  - `headers`: Content-Type application/json.
  - `body:json`: test payload.
  - `docs`: explanation of the endpoint purpose.
  - `example`: example of request and successful response.

---

## 5. Golden Rule for Utilities (`src/utils/`)

- **Prohibition of Inline or Local Helpers:** It is strictly prohibited to define auxiliary functions or generic validation, formatting, or parsing helpers (e.g., `validateBody<T>()`, date transformers, Zod validators, cryptographic helpers) within a controller file (`*.controller.ts`) or service file (`*.service.ts`).
- **Mandatory Location in `src/utils/`:** Any generic function likely to be used by more than one module or controller MUST be created within `src/utils/` (e.g., `validation.util.ts`, `crypto.util.ts`, `date.util.ts`) to ensure DRY, modularity, and cross-cutting reuse.

---

## 6. Backend Health, Concurrency, Caching & Pooling (Universal Standard)

> **NON-NEGOTIABLE HEALTH RULE:** Applies regardless of the language or framework used (TypeScript/Node/Bun, Go, Rust, Python, etc.).

1. **Concurrent vs. Sequential Execution (Anti-Waterfall & Anti N+1):**
   - **Waterfall effect is prohibited:** If multiple independent I/O or database queries are executed in the same handler/service, they MUST NEVER be executed sequentially one after another (`await q1; await q2;`). They must be executed in parallel (e.g., `Promise.all([q1, q2])` or native concurrency).
   - **I/O inside loops is prohibited (Anti N+1):** It is strictly prohibited to make database queries or internal API calls inside a `for`, `map`, or `forEach` loop. They must be aggregated using `IN (...)`, `JOIN`, or batching.
2. **Non-Blocking Event Loop:**
   - Every HTTP call must be asynchronous and non-blocking. CPU-bound intensive computation operations must be delegated to secondary threads (Worker Threads) or background async queues.
3. **Mandatory Connection Pooling:**
   - Every relational database client or driver (PostgreSQL, MySQL, SQLite/Turso, etc.) MUST be configured with an explicit Connection Pool (define `max`, `idleTimeout`, `connectionTimeout`). Opening/closing direct connections per HTTP request is prohibited.
4. **Strategic Caching (Cache-Aside & TTL):**
   - High-frequency read and low-frequency update data must be stored in memory (RAM / Redis) implementing the *Cache-Aside* pattern with a mandatory TTL (Time-To-Live).
5. **Concurrency Limits & Protections (Rate Limiting & Throttling):**
   - **Rate Limiting:** Every exposed API must have a request rate limiter per client/IP (e.g., Token Bucket / Redis Rate Limiter).
   - **Concurrency Throttling:** If multiple heavy calls are executed in parallel to the database or third-party services, they must be capped using a maximum concurrency control/semaphore to prevent socket and memory exhaustion.

---

## 7. Provider Adapter Pattern & Dependency Inversion (DIP)

> **THIRD-PARTY ABSTRACTION & DOMAIN GROUPING RULE:** No application service (`src/modules/**/*.service.ts`) should depend on or directly import types, names, or functions specific to a particular external provider or vendor (e.g., `calculateValhallaRoute`, `ValhallaTrip`, `StripeChargeDTO`, `TwilioSmsParams`).

1. **Domain-Grouped Provider Structure:**
   - All service providers are organized in `src/providers/<domain>/` (e.g., `src/providers/routing/`, `src/providers/payment/`, `src/providers/email/`).
   - The domain root exposes the domain-agnostic adapter layer (`services/`, `interfaces/`, `index.ts`).
2. **Concrete Provider Subdirectory (`src/providers/<domain>/providers/<vendor>/`):**
   - Concrete vendor/provider implementations for a domain reside in their `providers/<vendor>/` subdirectory (e.g., `src/providers/routing/providers/valhalla/`, `src/providers/payment/providers/stripe/`, `src/providers/email/providers/resend/`).
   - This prevents disorganized folder accumulation at the `src/providers/` root.
3. **Domain-Agnostic Adapter Layer (Port / Adapter):**
   - The adapter layer in `src/providers/<domain>/services/` receives requests from business modules, delegates to the active vendor implementation (`src/providers/<domain>/providers/<vendor>/`), and translates the response toward domain-agnostic DTOs.
4. **Transparent Provider Replacement (Zero Breakage):**
   - Adding or changing a provider (e.g., adding OSRM in `src/providers/routing/providers/osrm/`) MUST be transparent to the business layer (`src/modules/`), requiring zero changes in application services.

---

## 8. Mandatory Database Migrations & Autonomy Without MCP

> **PERSISTENCE & MIGRATION GOLDEN RULE:** Depending on third-party SQL executors or MCP database servers to alter schemas in production or development is prohibited.

1. **Mandatory Physical Migrations (`drizzle/*.sql` and `_journal.json`):**
   - Every change or addition to the ORM schema (`src/db/schema.ts`) MUST generate/create its corresponding SQL migration file within the migrations directory (`drizzle/XXXX_name.sql`) and update its index in `drizzle/meta/_journal.json`.
2. **Total Server Autonomy:**
   - Backend code must be 100% independent. During the application initialization process (`main.ts` / `runMigrations()`), the server executes automatic migrations against the target database (`migrate(db, { migrationsFolder: "./drizzle" })`), guaranteeing the same schema in any environment (Local, CI/CD, Staging, Production).
