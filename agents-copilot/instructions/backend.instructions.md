---
description: Backend Workflow & Clean Standards for Node.js, Bun, Fastify and Express
applyTo: "src/**/*.ts, src/**/*.js"
---

# Backend Workflow & Architecture Standards

> Universal standards for backend services, APIs, and microservices in Node.js, Bun, Fastify, and Express.

## 1. Runtime & Framework Invariants

- **Runtime**: **Bun** is the preferred high-performance JavaScript/TypeScript runtime.
- **Frameworks**: **Fastify** or **Express** depending on project requirements.
- **Architecture**: Strict vertical slicing and 3-tier layering:
  - `Controller / Handler`: Handles HTTP request validation and mapping.
  - `Service`: Encapsulates pure domain business rules.
  - `Repository / DB Provider`: Interfaces with the database (Drizzle, Prisma, SQL).

## 2. Result Pattern & Zero Unhandled Exceptions

- Services and domain functions NEVER throw raw exceptions.
- Always return typed Results: `{ success: true, data } | { success: false, error }`.
- Use a unified `sendResult` helper to serialize responses:
  - **Success (200 / 201):** `{ "message": "...", "data": T }`
  - **Failure (4xx / 5xx):** `{ "error": "ErrorCode" }`

## 3. Mandatory Bruno API Collections (`.bru`)

- Every HTTP route/endpoint created or modified MUST have a corresponding `.bru` collection file in `bruno/Public/` or `bruno/Private/`.
- Must include method, URL with `{{base_url}}`, headers, sample request body, and documentation.

## 4. Structured Logging & Observability

- Use **Pino** for all application logging.
- Prohibit `console.log` in production backend code.
- Never log raw passwords, full JWT tokens, or sensitive user data.
