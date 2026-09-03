---
name: bun
description: Comprehensive knowledge and best practices for developing with Bun (all-in-one JavaScript/TypeScript runtime, bundler, test runner, and package manager). Use when working with Bun commands, Bun APIs, Fastify/Elysia with Bun, Bun.serve, bun:test, Drizzle ORM on Bun, Bun.file, and bun shell scripts.
license: MIT
metadata:
  authors: "Antigravity Team"
  version: "1.0.0"
---

# ⚡ Bun Skill & Knowledge Guide

Bun is an all-in-one JavaScript and TypeScript runtime, bundler, test runner, and package manager designed for performance and developer ergonomics.

---

## 🚀 1. CLI Commands & Tooling

Default to using Bun instead of Node.js, npm, yarn, or pnpm when working in a Bun environment:

| Operation | Bun Command | Node / npm Equivalent |
|---|---|---|
| Run file | `bun <file.ts\|file.js>` | `node <file.js>` / `ts-node <file.ts>` |
| Run with watch | `bun --watch <file.ts>` | `nodemon <file.ts>` / `tsx watch` |
| Run with HMR | `bun --hot <file.ts>` | Live reload with memory preserve |
| Run script | `bun run <script>` | `npm run <script>` / `pnpm <script>` |
| Execute binary | `bunx <package> <cmd>` | `npx <package> <cmd>` |
| Install dependencies | `bun install` / `bun add <pkg>` | `pnpm install` / `npm install` |
| Add dev dependency | `bun add -d <pkg>` | `pnpm add -D <pkg>` |
| Run test suite | `bun test` | `vitest` / `jest` |
| Bundle code | `bun build <file.ts> --outdir dist` | `esbuild` / `webpack` |

> **Tip:** Bun automatically loads `.env` files (`.env`, `.env.local`, `.env.development`, `.env.production`) without needing packages like `dotenv`.

---

## 🛠️ 2. Core Bun Native APIs

### 2.1 HTTP Server & WebSockets (`Bun.serve`)
```typescript
const server = Bun.serve({
  port: Number(process.env.PORT) || 3000,
  routes: {
    '/health': {
      GET: () => Response.json({ status: 'ok', time: new Date().toISOString() }),
    },
    '/api/users/:id': {
      GET: (req) => Response.json({ id: req.params.id }),
    },
  },
  websocket: {
    open: (ws) => ws.send('Connected to Bun WS!'),
    message: (ws, message) => ws.send(`Echo: ${message}`),
    close: (ws) => console.log('Client disconnected'),
  },
  fetch: (req) => new Response('Not Found', { status: 404 }),
});
```

### 2.2 File System I/O (`Bun.file`)
High-performance native file operations:
```typescript
// Reading
const file = Bun.file('data.json');
const text = await file.text();
const json = await file.json();
const exists = await file.exists();

// Writing
await Bun.write('output.txt', 'Hello Bun!');
await Bun.write('response.json', JSON.stringify({ ok: true }, null, 2));
```

### 2.3 Password Hashing (`Bun.password`)
Native Argon2id and bcrypt without native binding compilation issues:
```typescript
// Argon2id (Default & Recommended)
const hash = await Bun.password.hash('MySecretPassword123!', {
  algorithm: 'argon2id',
  memoryCost: 65536,
  timeCost: 3,
});

const isValid = await Bun.password.verify('MySecretPassword123!', hash);
```

### 2.4 Subprocesses & Shell (`Bun.$`)
Cross-platform shell scripting without `execa` or child_process boilerplate:
```typescript
import { $ } from 'bun';

const branch = await $`git rev-parse --abbrev-ref HEAD`.text();
await $`echo "Active branch is: ${branch.trim()}"`;
```

---

## 🧪 3. Testing with `bun:test`

Fast, built-in Jest/Vitest compatible test runner:

```typescript
import { describe, it, expect, beforeEach, mock, spyOn } from 'bun:test';

describe('Calculator Service', () => {
  it('should sum numbers accurately', () => {
    expect(2 + 2).toBe(4);
  });

  it('should support async tests and mocking', async () => {
    const fetchMock = mock(async () => ({ ok: true }));
    const result = await fetchMock();
    expect(result.ok).toBe(true);
    expect(fetchMock).toHaveBeenCalledTimes(1);
  });
});
```

Execute tests:
```bash
bun test                    # Run all *.test.ts files
bun test --watch            # Watch mode
bun test --coverage         # Code coverage report
```

---

## 🗄️ 4. Databases & ORMs in Bun

1. **PostgreSQL / PostGIS with Drizzle ORM:**
   Use standard driver `postgres` or `@neondatabase/serverless` with `drizzle-orm`.
   ```typescript
   import { drizzle } from 'drizzle-orm/postgres-js';
   import postgres from 'postgres';
   import * as schema from './schema';

   const client = postgres(process.env.DATABASE_URL!);
   export const db = drizzle(client, { schema });
   ```
2. **SQLite (`bun:sqlite`):**
   Built-in zero-dependency SQLite driver:
   ```typescript
   import { Database } from 'bun:sqlite';
   const db = new Database('mydb.sqlite');
   ```
3. **Redis:**
   Use `ioredis` or native Redis connectors.

---

## 📦 5. Framework Integration & Best Practices

- **Fastify on Bun:** Works out of the box with Node compatibility layer. Always use latest Fastify 5+ or 4.x.
- **Pure Functional TypeScript:** Favor pure functions, explicit Result types, and modular vertical architecture.
- **Type Checking:** Run `tsc --noEmit` alongside `bun` for compile-time validation.
