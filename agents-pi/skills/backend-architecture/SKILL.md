---
name: backend-architecture
description: Backend architecture and clean workflow for Node.js, Bun, Fastify, and Express. Includes public vs private module segregation, Result Pattern HTTP handlers, structured Pino logging, and Bruno API collections.
license: MIT
compatibility: opencode
metadata:
  domain: backend
  frameworks: fastify,express,bun
---

# Backend Architecture & Clean Standards

Golden standards for resilient, observable, and strictly typed backend development across **Fastify**, **Express**, and **Bun**.

## 1. Core Backend Invariants

1. **Modular Route Segregation**:
   - `src/modules/public/`: Unauthenticated routes (Login, Register, Webhooks, Public assets).
   - `src/modules/private/`: Authenticated routes protected by session or JWT token middleware.
   - `src/providers/`: Cross-cutting infrastructure and adapter singletons (Database, Logger, Cache, External SDKs).
2. **Result Pattern in Services**: Domain services must never throw exceptions. They return a strongly typed `Result<T, AppError>`.
3. **Unified Response Schema**: All HTTP responses follow a deterministic envelope:
   - Success (`200`/`201`): `{ "message": "...", "data": T }`
   - Failure (`4xx`/`5xx`): `{ "error": "ErrorCode", "message": "..." }`
4. **Structured Pino Logging**: No `console.log`. Use structured Pino logger with log sanitization (strip tokens, passwords, credit cards).
5. **Mandatory Bruno Testing (`.bru`)**: Every endpoint must be documented with an executable Bruno file in `bruno/Public/` or `bruno/Private/`.

---

## 2. Directory Layout

```text
src/
├── app.ts                  # Framework bootstrap & plugins
├── server.ts               # Listener entry point
├── providers/              # Infrastructure adapters & cross-cutting tools
│   ├── database.provider.ts
│   ├── logger.provider.ts
│   └── result.provider.ts
├── modules/
│   ├── public/
│   │   └── Auth/
│   │       ├── auth.controller.ts
│   │       ├── auth.service.ts
│   │       ├── auth.routes.ts
│   │       └── auth.schemas.ts
│   └── private/
│       └── Accounts/
│           ├── accounts.controller.ts
│           ├── accounts.service.ts
│           └── accounts.routes.ts
└── utils/                  # Pure, stateless utilities
```

---

## 3. Fastify Route & Controller Example

```typescript
// Controller leveraging sendResult
import { FastifyReply, FastifyRequest } from 'fastify';
import { loginUser } from './auth.service';
import { sendResult } from 'src/providers/result.provider';

export const loginHandler = async (req: FastifyRequest<{ Body: LoginDTO }>, reply: FastifyReply) => {
  const result = await loginUser(req.body);
  return sendResult(reply, result, 200);
};
```

---

## 4. Bruno Collection Standard (`.bru`)

For each endpoint, save a file `bruno/Private/Accounts/GetAccount.bru`:

```text
meta {
  name: Get Account by ID
  type: http
  seq: 1
}

get {
  url: {{base_url}}/api/accounts/acc_123
  body: none
  auth: bearer
}

auth:bearer {
  token: {{jwt_token}}
}

docs {
  Retrieves account profile for the authenticated tenant.
}
```
