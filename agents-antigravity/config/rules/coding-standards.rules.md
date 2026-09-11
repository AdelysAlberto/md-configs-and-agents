---
description: "Universal coding standards, zero-tolerance anti-patterns, pure functional paradigm, and engineering rigor"
trigger: model_decision
applyTo: "src/**/*.ts, src/**/*.tsx, src/**/*.js, src/**/*.jsx, src/**/*.go, src/**/*.py, **"
---

# Universal Coding Standards & Anti-Pattern Elimination

> **Universal Invariant**: These standards and anti-pattern bans apply across all languages, frameworks, architectural layers (frontend, backend, workers, lambdas, mobile), and transport protocols (REST, WebSockets, Pub/Sub, gRPC).

---

## 1. Radical Simplicity & Technical Honesty

- **Simplicity First**: Always prioritize the simplest, most readable solution. Avoid over-engineering, speculative layers, abstractions for hypothetical future use cases, or unnecessary complexity.
- **Technical Honesty ("Say I don't know")**: Never invent solutions, fake APIs, or hallucinate non-existent features. If a requirement is ambiguous or a capability is unverified, acknowledge it explicitly and ask rather than guessing.

---

## 2. Universal Anti-Pattern Invariants (Strict Zero-Tolerance Policy)

### 2.1. Pub/Sub Loop Isolation & Publisher Deduplication (Zero Double Fan-Out)
- **Anti-Pattern**: A node/process publishes an event to a broker or WebSocket hub and simultaneously subscribes to the same topic/stream without sender filtering, receiving its own message back and executing duplicate work (self-echo).
- **Mandatory Rule**: Publishers must NEVER process their own emitted events unless explicitly designed with loopback deduplication.
- **Enforcement**: Include an origin/sender ID (`senderId`, `nodeId`) in message envelopes, use `noLocal: true` flags, or strictly segregate ingest channels from broadcast/egress channels.

```typescript
// ❌ BAD: Publishing and processing the same event on the same node
pubsub.publish("events:chat", { messageId, text });
pubsub.subscribe("events:chat", (msg) => processChatMessage(msg)); // Self-echo!

// ✅ GOOD: Sender-filtered or segregated ingest/egress
pubsub.publish("events:chat:ingest", { messageId, text, senderId: currentWorkerId });
pubsub.subscribe("events:chat:broadcast", (msg) => {
  if (msg.senderId === currentWorkerId) return; // Deduplicated
  processChatMessage(msg);
});
```

### 2.2. Scoped Entity Subscriptions (Zero Unbounded Wildcards)
- **Anti-Pattern**: Subscribing to wildcard or catch-all topics (e.g. `stream:*`, `room:*`, `#`, `events.*`) in client streams or tenant-scoped workers, causing message storm amplification, memory bloat, and cross-tenant data leaks.
- **Mandatory Rule**: All subscriptions MUST be strictly scoped to specific entity IDs, rooms, or tenant boundaries (e.g. `stream:room:123`, `user:456`).
- **Enforcement**: Wildcard subscriptions on shared event buses or WebSocket connections are strictly prohibited for client/tenant-level streams.

```typescript
// ❌ BAD: Wildcard stream amplification
socket.subscribe("caravan:stream:*"); // Amplification across all caravans

// ✅ GOOD: Strictly scoped entity subscription
socket.subscribe(`caravan:stream:${caravanId}`);
```

### 2.3. Bounded In-Memory State & Explicit Backpressure (Zero Unbounded Maps)
- **Anti-Pattern**: Accumulating state in memory (global `Map`, `Set`, `Array`, in-process caches, queue buffers) without capacity caps, TTLs, or flow control, leading to memory leaks and OOM (Out Of Memory) crashes.
- **Mandatory Rule**: Every in-memory collection MUST have bounded capacity (Max Items / LRU eviction), time-to-live (TTL), and backpressure / rate-limiting mechanisms. Critical shared state MUST reside in a persistent/distributed store (Redis, DB).
- **Enforcement**: Never use unbounded global maps as primary state stores.

```typescript
// ❌ BAD: Unbounded global map accumulating items forever
const activeSessions = new Map<string, SessionData>(); // Leaks memory & lost on crash

// ✅ GOOD: Bounded LRU cache with TTL or distributed store with expiration
import { LRUCache } from "lru-cache";
const activeSessions = new LRUCache<string, SessionData>({
  max: 5000,
  ttl: 1000 * 60 * 30, // 30 mins
});
```

### 2.4. Zero Secrets in URLs / Query Parameters
- **Anti-Pattern**: Passing API keys, tokens, session secrets, passwords, or PII in URL query parameters (`?token=xyz`, `?apiKey=123`), leaking credentials in access logs, reverse proxies, browser history, APM traces, and HTTP Referer headers.
- **Mandatory Rule**: Sensitive credentials and tokens MUST NEVER travel via URL query strings or path parameters.
- **Enforcement**: Transmit credentials exclusively via standard headers (`Authorization: Bearer <token>`, `X-API-Key`), encrypted request bodies (POST/PUT), or secure `HttpOnly; SameSite=Strict; Secure` cookies. WebSocket handshakes must authenticate via ticket exchange, headers, or initial auth frames.

```typescript
// ❌ BAD: Passing sensitive tokens in query string
fetch(`/api/v1/trips?apiKey=${SECRET_API_KEY}&token=${USER_TOKEN}`);

// ✅ GOOD: Standard Authorization header
fetch(`/api/v1/trips`, {
  headers: {
    Authorization: `Bearer ${userToken}`,
    "X-API-Key": apiKey,
  },
});
```

### 2.5. Deterministic Lifecycle & Mandatory Expiration (Zero Immortal State)
- **Anti-Pattern**: Ephemeral domain state (sessions, cache entries, distributed locks, OTPs, temporary rooms, WebSocket listeners, intervals) created without explicit expiration or cleanup, causing zombie locks, orphaned resources, and memory leaks.
- **Mandatory Rule**: Every ephemeral state, session, distributed lock, timer, and subscription MUST have an explicit TTL, heartbeat/reaper cleanup, and teardown logic on disconnect/unmount.
- **Enforcement**: Distributed locks must always have an auto-release TTL. Event listeners and intervals must be unregistered upon destruction.

```typescript
// ❌ BAD: Immortal key without TTL and lock without expiration
await redis.set(`lock:booking:${id}`, "locked"); // Can deadlock forever on failure

// ✅ GOOD: Atomic lock with mandatory TTL expiration
await redis.set(`lock:booking:${id}`, workerId, "PX", 5000, "NX");
```

### 2.6. Enforced Business Rules & Invariant Guarding (Zero Phantom Rules)
- **Anti-Pattern**: Defining business constraints (e.g., `maxMembers`, paywall limits, tier quotas, balance checks, status transition rules) in database schemas, DDL comments, or UI screens, but failing to validate and enforce them in the business/service layer.
- **Mandatory Rule**: Business rules and invariants MUST be strictly guarded and validated in domain service logic with transactional atomicity.
- **Enforcement**: Never rely on database constraints or frontend UI checks alone to protect domain rules. Assert constraints in service handlers and fail fast if violated.

```typescript
// ❌ BAD: Phantom business rule (assumes UI or schema prevents exceeding limit)
export const joinCaravan = async (caravanId: string, userId: string) => {
  return db.caravanMembers.insert({ caravanId, userId }); // No check for maxMembers!
};

// ✅ GOOD: Guarded invariant with transactional atomic enforcement
export const joinCaravan = async (caravanId: string, userId: string): Promise<Result<void>> => {
  const caravan = await getCaravanById(caravanId);
  if (!caravan) return Result.fail("Caravan not found");
  
  const currentMembersCount = await countCaravanMembers(caravanId);
  if (currentMembersCount >= caravan.maxMembers) {
    return Result.fail("Caravan capacity reached"); // Explicit business rule enforcement
  }
  
  return db.caravanMembers.insert({ caravanId, userId });
};
```

### 2.7. Idempotent State Mutations & Deduplication
- **Anti-Pattern**: Writing mutation endpoints (creation, payment, joining, status transition, webhook handlers) that execute duplicate side effects or corrupt data when retried by network glitches or rapid user clicks.
- **Mandatory Rule**: All state-changing write operations MUST be idempotent and safely handle retries.
- **Enforcement**: Implement idempotency keys (`Idempotency-Key` header), database unique constraints, atomic `upsert` / conditional writes, or atomic deduplication locks.

```typescript
// ❌ BAD: Blind insert without idempotency or deduplication
export const recordTrip = async (tripData: TripData) => {
  return db.trips.insert(tripData); // Network retry creates duplicate trip!
};

// ✅ GOOD: Idempotency key or unique transactional constraint
export const recordTrip = async (tripData: TripData, idempotencyKey: string): Promise<Result<Trip>> => {
  return db.transaction(async (tx) => {
    const existing = await tx.idempotencyKeys.find(idempotencyKey);
    if (existing) return Result.ok(existing.responsePayload);
    
    const newTrip = await tx.trips.insert(tripData);
    await tx.idempotencyKeys.insert({ key: idempotencyKey, responsePayload: newTrip });
    return Result.ok(newTrip);
  });
};
```

### 2.8. Zero Silent Swallowing of Errors (Explicit Failure Propagation)
- **Anti-Pattern**: Using empty `catch` blocks (`catch (e) {}`), ignoring error return values, or converting infrastructure/network failures into silent nulls/no-ops.
- **Mandatory Rule**: NEVER silently swallow errors or exceptions.
- **Enforcement**: Every error must be explicitly logged with context, converted to an explicit Result error contract (`Result.fail(error)`), or escalated to an appropriate boundary.

```typescript
// ❌ BAD: Empty catch swallowing infrastructure failure
try {
  await syncDataWithExternalService(payload);
} catch (e) {} // Silent death, impossible to diagnose

// ✅ GOOD: Explicit logging and Result propagation
try {
  await syncDataWithExternalService(payload);
  return Result.ok();
} catch (error) {
  logger.error("[ExternalSync] Infrastructure failure during sync", { error, payloadId: payload.id });
  return Result.fail(new ExternalServiceError("Failed to sync payload", { cause: error }));
}
```

### 2.9. Batch Operations Over Iterative N+1 Loops
- **Anti-Pattern**: Executing database queries, remote HTTP requests, or I/O calls inside iterative loops (`map`, `forEach`, `for`) for collection elements.
- **Mandatory Rule**: Never perform individual queries or remote calls per item inside loops.
- **Enforcement**: Use batching (`WHERE id IN (...)`), joins, bulk endpoints, aggregations, or DataLoader patterns to resolve related data in `O(1)` operations.

```typescript
// ❌ BAD: N+1 query loop
const users = await db.users.findMany();
const usersWithProfiles = await Promise.all(
  users.map(async (user) => {
    const profile = await db.profiles.findOne({ userId: user.id }); // N queries!
    return { ...user, profile };
  })
);

// ✅ GOOD: Single batch query / Join
const usersWithProfiles = await db.users.findMany({
  include: { profile: true }, // 1 query with JOIN or bulk IN
});
```

### 2.10. Omni-Channel Boundary Validation & Zero-Trust Ingress
- **Anti-Pattern**: Validating incoming payloads only on REST routes while treating WebSocket messages, SSE events, Pub/Sub messages, Webhooks, or RPC calls as trusted and processing raw inputs.
- **Mandatory Rule**: EVERY entry point into the system is an untrusted boundary.
- **Enforcement**: Inbound payloads on WebSockets, event consumers, webhooks, and queues MUST undergo strict schema validation (e.g. Zod, JSON Schema) and authorization before reaching business logic.

```typescript
// ❌ BAD: Trusting WebSocket payload directly without schema validation
ws.on("message", (raw) => {
  const data = JSON.parse(raw);
  executeUserCommand(data.userId, data.action, data.payload); // Blind execution!
});

// ✅ GOOD: Schema validation and auth verification on WS message ingress
ws.on("message", (raw) => {
  const parseResult = WebSocketMessageSchema.safeParse(JSON.parse(raw));
  if (!parseResult.success) {
    return ws.send(JSON.stringify({ error: "Invalid payload schema", details: parseResult.error }));
  }
  if (parseResult.data.userId !== ws.authenticatedUserId) {
    return ws.send(JSON.stringify({ error: "Unauthorized sender" }));
  }
  executeUserCommand(parseResult.data.userId, parseResult.data.action, parseResult.data.payload);
});
```

---

## 3. Functional Programming Invariant (Zero OOP)

This codebase is **strictly functional**. Object-oriented structures are prohibited.

- **Banned**: `class`, `this`, `constructor`, `extends` (on classes), `implements`.
- **Allowed**: Pure functions, closures, function composition, factory functions.

```typescript
// ❌ BAD: OOP / Class-based
class UserService {
  constructor(private http: HttpClient) {}
  getUser(id: string) { return this.http.get(`/users/${id}`); }
}

// ✅ GOOD: Pure functional
export const getUser = (http: HttpClient) => (id: string) => http.get(`/users/${id}`);
```

---

## 4. Pinned Exact Dependencies (Zero Caret / Tilde)

In `package.json`, wildcards (`^`, `~`) are strictly forbidden. Always pin exact versions to ensure deterministic, reproducible builds.

```json
// ❌ BAD
"dependencies": {
  "react": "^19.0.0",
  "zustand": "~5.0.0"
}

// ✅ GOOD
"dependencies": {
  "react": "19.0.0",
  "zustand": "5.0.3"
}
```

---

## 5. TypeScript Rules

- **Zero `any`**: Use explicit generics or `unknown` with type guards.
- **Types vs Interfaces**: Use `interface` for extensible object shapes and `type` for unions/primitives.
- **Enums Prohibited**: Use TypeScript union literals (e.g. `type Status = 'idle' | 'loading' | 'success' | 'error'`).
- **No `React.FC`**: Type props directly in function signatures.

---

## 6. Lookup Dictionary Pattern Over `switch` Statement

Prefer declarative Lookup Dictionaries (`Record<string, T>`) over imperative `switch(string)` or multiple `if/else` conditionals for factory functions, state mappers, or strategy selection.

- **Performance**: Constant `O(1)` hash lookup vs `O(N)` sequential string comparison.
- **Maintainability (SOLID - Open/Closed)**: Adding new keys is declarative and risk-free without mutating control flow logic.

```typescript
// ❌ BAD: Imperative switch statement
switch (key) {
  case "openai":
  case "chatgpt":
    return openaiProvider;
  case "gemini":
  default:
    return geminiProvider;
}

// ✅ GOOD: Declarative Lookup Dictionary (O(1))
const PROVIDER_MAP: Record<string, AIProviderInterface> = {
  openai: openaiProvider,
  chatgpt: openaiProvider,
  gemini: geminiProvider,
};

export const getAIProvider = (key?: string): AIProviderInterface => {
  return PROVIDER_MAP[key?.toLowerCase().trim() || "gemini"] ?? geminiProvider;
};
```

---

## 7. Strict DRY Invariant (Zero Duplication of Prompts, Logic & Strings)

Never duplicate system prompts, magic strings, complex regex, configuration objects, or validation logic across multiple files.

- **System Prompts & LLM Schemas**: Extract into single shared constants (e.g. `constants/aiPrompts.constant.ts`) and import across adapters.
- **Environment Variables & Config**: Use `configEnvs` from `src/utils/env.ts` instead of scatter-reading `process.env.*`.

```typescript
// ❌ BAD: Duplicating system prompts or env reads across adapters
const systemPrompt = "Eres un copiloto y planificador de rutas..."; // repeated in 4 files
const key = process.env.API_KEY;

// ✅ GOOD: Single source of truth constants & configEnvs
import { AI_ROUTE_SYSTEM_PROMPT } from "../constants/aiPrompts.constant";
import { configEnvs } from "../../../utils";
```

---

## 8. Pure Reusable Utilities Invariant (`src/utils/`)

Pure helper functions (math calculations, geometry processing, string formatters, date manipulators, transformations) **MUST** reside in `src/utils/`.

- **Pure & Deterministic**: Functions in `src/utils/` must be pure (same inputs always produce same outputs) without hidden side effects.
- **Zero Coupling**: Utilities must be independent of application state, HTTP frameworks, or specific UI components.
- **DRY Reusability**: Whenever a data transformation logic is needed in more than one place, extract it into `src/utils/<utility-name>.util.ts`.

---

## 9. Deterministic Pre-Delivery Gate

Before marking any technical task as done, verify:

```bash
bun run biome:check && bun run check && bun test
# OR
pnpm fix && pnpm tsc --noEmit && pnpm test
```
