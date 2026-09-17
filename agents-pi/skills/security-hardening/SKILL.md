---
name: security-hardening
description: Security hardening standards, OWASP Top 10 mitigation, API rate limiting, input sanitization, secure cookies, token handling, and frontend shielding. Use when reviewing security, authentication, or sensitive endpoints.
license: MIT
compatibility: opencode
metadata:
  domain: security
  standards: owasp
---

# Security Hardening & Endpoint Protection

Pragmatic, high-impact security standards for web and API applications. Enforces **OWASP Top 10 mitigation**, **endpoint shielding**, and **credential isolation**.

## 1. Core Security Invariants

1. **Defense in Depth with Simplicity**: Apply security at the network, framework, and application boundaries without introducing unnecessary architectural bureaucracy.
2. **Never Trust Client Input**: Validate and sanitize all query parameters, request bodies, and headers at the API boundary using schema validation (Zod, TypeBox).
3. **Zero Secrets in Source or Client Storage**: Never commit API keys or private certificates. Never store JWTs, access tokens, or sensitive user data in `localStorage` or unencrypted client stores.
4. **Secure Transport & Cookies**: Always use HTTPS. Session cookies must be marked `HttpOnly`, `Secure`, and `SameSite=Strict` (or `Lax`).
5. **Sanitized Error Responses**: Never leak database errors, internal stack traces, or environment paths to HTTP clients in production.

---

## 2. API Endpoint Protection & Rate Limiting

Apply strict rate limits to sensitive routes (authentication, registrations, password resets, payment webhooks):

```typescript
// Example: Route-level rate limiting in Fastify/Express
import rateLimit from '@fastify/rate-limit';

await fastify.register(rateLimit, {
  global: false, // Apply selectively to sensitive endpoints
});

fastify.post('/api/auth/login', {
  config: {
    rateLimit: {
      max: 5,
      timeWindow: '1 minute',
      errorResponseBuilder: () => ({
        success: false,
        error: { code: 'RATE_LIMIT_EXCEEDED', message: 'Too many login attempts. Please wait 1 minute.' },
      }),
    },
  },
}, async (request, reply) => {
  // Login handler
});
```

---

## 3. OWASP Top 10 Mitigations

| Vulnerability | Attack Vector | Mandatory Defense |
| :--- | :--- | :--- |
| **SQL Injection (SQLi)** | Concatenated SQL strings | Use parameterized queries exclusively via ORM (Drizzle, Prisma) or prepared statements. Never format strings into SQL. |
| **Cross-Site Scripting (XSS)** | Injected HTML/script tags | Avoid `dangerouslySetInnerHTML`. Sanitize user markup with DOMPurify when rendering rich text. |
| **Cross-Site Request Forgery (CSRF)** | Unauthorized commands from browser | Use `SameSite=Strict` cookies and CSRF anti-forgery tokens on state-modifying requests (`POST`, `PUT`, `DELETE`). |
| **Broken Object Level Auth (BOLA)** | Modifying `userId` in URL | Verify ownership and tenant scope on every query: `WHERE id = :id AND tenant_id = :currentTenant`. |
| **Security Misconfiguration** | Missing HTTP headers | Set secure headers using `helmet`: `Content-Security-Policy`, `X-Frame-Options: DENY`, `X-Content-Type-Options: nosniff`. |

---

## 4. Token & Session Hygiene

- **Refresh Tokens**: Store in secure, `HttpOnly` cookies rotated upon each generation.
- **Access Tokens**: Short-lived (e.g. 5–15 minutes) kept in volatile client memory (or isolated closures).
- **Session Revocation**: Maintain a Redis-backed blacklist or token version counter to instantly invalidate compromised accounts.

---

## 5. Frontend Shielding

- Strip debug tooling and console logs from production builds.
- Protect client routes with both local layout guards AND authoritative server-side 401/403 responses.
- Implement CSP (Content Security Policy) to restrict scripts only to trusted origins.
