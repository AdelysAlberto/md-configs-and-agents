---
name: backend_workflow_standards
description: Backend Workflow & Clean Standards for Node.js, Bun, Fastify and Express. Use when working on files matching: src/**/*.ts, src/providers/**, src/modules/**.
metadata:
  hermes:
    tags: [team-pinky, coding-standards]
    category: engineering
---

# Rule: Backend Workflow & Clean Standards (Adely's Golden Standards)

> **MANDATORY BACKEND DEVELOPMENT STANDARD:** Applies to all backend development in Node.js, Bun, Fastify, and Express.

---

## 1. 📁 Directory Structure (Public vs Private & Providers)

- **`src/providers/`**: Contains only infrastructure providers and cross-cutting services:
  - `logger.provider.ts` (Structured Pino Logger).
  - `result.provider.ts` (Result Pattern + `sendResult` helper).
  - `appError.provider.ts` (Safety net for the global errorHandler).
  - `database.provider.ts` (ORM / Query Builder).
  - `redis.provider.ts`, etc.
- **`src/modules/public/`**: Contains the modules and routes that do NOT require prior authentication (e.g. `Auth/` with login, register, email validation).
- **`src/modules/private/`**: Contains the protected modules and routes that require a JWT token or active session (e.g. `Accounts/`, `Garage/`, `Routes/`, etc.).

---

## 2. ⚡ Result Pattern & Unified HTTP Responses

1. **Zero Exceptions in Services:**
   - Services NEVER throw exceptions (`throw`). They always return `Promise<Result<T>>`.
   - They use `ResponseOk(data)` for success and `ResponseFail.X("ErrorCode")` for failures (where `X` represents the HTTP error type: `BadRequest`, `Unauthorized`, `NotFound`, `UnprocessableEntity`, `Conflict`, `Internal`, etc.).
2. **`sendResult` Helper in Handlers/Controllers:**
   - Every HTTP response sent by the server strictly follows the same JSON schema:
     - **Success (200 / 201):** `{ "message": "...", "data": T }`
     - **Failure (4xx / 5xx):** `{ "error": "ErrorCode" }`

---

## 3. 🪵 Structured Logs (`src/providers/logger.provider.ts`)

- Exclusively use Pino Logger to emit server logs (`logger.info`, `logger.warn`, `logger.error`).
- Log **only strictly necessary information** (service startup, critical failures with context, key operations).
- Structured JSON output in production and formatted (pretty) in development.
- Logging passwords, full JWT tokens, or sensitive personal data is strictly prohibited.

---

## 🧪 4. Mandatory Bruno Test Collection (`.bru`)

- For each HTTP endpoint created or modified, a `.bru` file **MUST** be created in the corresponding `bruno/` folder (`bruno/Public/` or `bruno/Private/`).
- The `.bru` file must respect Bruno's complete structure:
  - `meta`: name, sequence, and type (`http`).
  - `method` (get/post/put/delete): URL with the `{{base_url}}` variable.
  - `headers`: Content-Type application/json.
  - `body:json`: test payload.
  - `docs`: explanation of the endpoint's purpose.
  - `example`: example of request and successful response.

---

## 🛠️ 5. Golden Rule of Utilities (`src/utils/`)

- **Prohibition of Inline or Local Helpers:** Defining generic helper or auxiliary validation, formatting, or parsing functions (e.g. `validateBody<T>()`, date transformers, Zod validators, cryptographic helpers) inside a controller (`*.controller.ts`) or service (`*.service.ts`) file is strictly prohibited.
- **Mandatory Location in `src/utils/`:** Any generic function likely to be used by more than one module or controller MUST be created obligatorily inside `src/utils/` (e.g: `validation.util.ts`, `crypto.util.ts`, `date.util.ts`) to guarantee DRY, modularity, and cross-cutting reuse.

---

## 🏥 6. Backend Health, Concurrency, Caching & Pooling (Universal Norm)

> **NON-NEGOTIABLE HEALTH RULE:** Applies regardless of the language or framework used (TypeScript/Node/Bun, Go, Rust, Python, etc.).

1. **Concurrent vs. Sequential Execution (Anti-Waterfall & Anti N+1):**
   - **Waterfall effect prohibited:** If a single handler/service executes multiple I/O or database queries that are independent of each other, they **MUST NEVER** run sequentially one after another (`await q1; await q2;`). They must run in parallel (e.g. `Promise.all([q1, q2])` or native concurrency).
   - **I/O inside loops prohibited (Anti N+1):** Performing DB queries or internal API calls inside a `for`, `map`, or `forEach` loop is strictly prohibited. They must be aggregated via `IN (...)`, `JOIN`, or batching.
2. **No Main Thread Blocking (Non-Blocking Event Loop):**
   - Every HTTP call must be asynchronous and non-blocking. CPU-intensive operations must be delegated to secondary threads (Worker Threads) or asynchronous background queues.
3. **Mandatory Connection Pooling:**
   - Every relational database client or driver (PostgreSQL, MySQL, SQLite/Turso, etc.) **MUST** be configured with an explicit Connection Pool (define `max`, `idleTimeout`, `connectionTimeout`). Opening/closing direct connections per HTTP request is prohibited.
4. **Strategic Caching (Cache-Aside & TTL):**
   - High read frequency and low update frequency data must be stored in memory (RAM / Redis) implementing the *Cache-Aside* pattern with a mandatory TTL (Time-To-Live).
5. **Concurrency Limits & Protections (Rate Limiting & Throttling):**
   - **Rate Limiting:** Every exposed API must have a per-client/IP request rate limiter (e.g. Token Bucket / Redis Rate Limiter).
   - **Concurrency Throttling:** If multiple heavy calls run in parallel toward the DB or third-party services, they must be bounded via a maximum concurrency control/semaphore to avoid exhausting sockets and memory.
---

## 7. 🔌 Provider Adapter Pattern & Dependency Inversion (DIP)

> **THIRD-PARTY ABSTRACTION AND DOMAIN GROUPING RULE:** No application service (`src/modules/**/*.service.ts`) may depend on or directly import specific types, names, or functions from a concrete external provider or vendor (e.g. `calculateValhallaRoute`, `ValhallaTrip`, `StripeChargeDTO`, `TwilioSmsParams`).

1. **Provider Structure Grouped by Domain:**
   - All service providers are organized in `src/providers/<domain>/` (e.g: `src/providers/routing/`, `src/providers/payment/`, `src/providers/email/`).
   - The domain root exposes the agnostic adapter layer (`services/`, `interfaces/`, `index.ts`).
2. **Concrete Providers Subdirectory (`src/providers/<domain>/providers/<vendor>/`):**
   - Concrete implementations of a domain's providers/vendors reside in their `providers/<vendor>/` subdirectory (e.g: `src/providers/routing/providers/valhalla/`, `src/providers/payment/providers/stripe/`, `src/providers/email/providers/resend/`).
   - This avoids the messy accumulation of folders at the root of `src/providers/`.
3. **Agnostic Adapter Layer (Port / Adapter):**
   - The adapter layer in `src/providers/<domain>/services/` receives requests from business modules, delegates to the active vendor implementation (`src/providers/<domain>/providers/<vendor>/`), and translates the response into agnostic DTOs.
4. **Transparent Provider Replacement (Zero Breakage):**
   - Adding or changing a provider (e.g. adding OSRM in `src/providers/routing/providers/osrm/`) **MUST** be transparent for the business layer (`src/modules/`), requiring zero changes in application services.

---

## 8. 🗄️ Mandatory Database Migrations and Autonomy without MCP

> **GOLDEN RULE OF PERSISTENCE AND MIGRATIONS:** Depending on third-party SQL executors or database MCP servers to alter schemas in production or development is prohibited.

1. **Mandatory Physical Migrations (`drizzle/*.sql` and `_journal.json`):**
   - Any change or addition to the ORM schema (`src/db/schema.ts`) **MUST** generate/create its corresponding SQL migration file within the migrations directory (`drizzle/XXXX_name.sql`) and update its index in `drizzle/meta/_journal.json`.
2. **Full Server Autonomy:**
   - The backend code must be 100% independent. In the application initialization process (`main.ts` / `runMigrations()`), the server runs automatic migrations on the target database (`migrate(db, { migrationsFolder: "./drizzle" })`), guaranteeing the same schema in any environment (Local, CI/CD, Staging, Production).
