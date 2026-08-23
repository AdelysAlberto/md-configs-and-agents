---
applyTo: "src/**"
---

# Project Context — r365-core Business Domain

## Service Identity

- **Service**: `r365-core`
- **Domain**: FinTech — user accounts, profiles, contacts, KYC orchestration, authentication, audit
- **Exclusions**: NO balance logic, fee calculations, or transactions — delegate to `r365-wallet`

## Priorities

1. Account security & authentication (tokens, refresh rotation, MFA)
2. Robust contact flows (request, accept, reject, block) with auditable events
3. Strict input validation and contract tests (OpenAPI)
4. Observability and immutable audit for critical actions

## Core Models

| Model | Key Fields |
|-------|-----------|
| **User** | `id`, `email` (normalized), `emailVerified`, `passwordHash` (argon2), `firstName`, `lastName`, `kycStatus`, `createdAt` |
| **Contact** | `id`, `ownerId`, `contactUserId`, `status` (pending\|accepted\|rejected\|blocked), `alias?`, `isFavorite?`, `createdAt` |

## API Surface

- **Auth**: `POST /auth/login`, `POST /auth/refresh`, `POST /auth/logout`
- **Users**: `POST /users`, `GET /users/:id`, `PATCH /users/:id`
- **Contacts**: `POST /contacts`, `POST /contacts/:id/accept`, `POST /contacts/:id/reject`, `GET /contacts`

Maintain OpenAPI specs in `src/docs/` — update with every API change.

## Security (FinTech — Mandatory)

| Area | Requirement |
|------|------------|
| **Auth** | Short-lived access tokens, rotating refresh tokens, MFA for sensitive ops |
| **Passwords** | argon2id hash, complexity policies, throttling |
| **Encryption** | TLS 1.2+, field-level encryption for sensitive data at rest |
| **Secrets** | Secret manager (Vault/KMS), never committed, rotation audited |
| **Least Privilege** | DB credentials with minimal permissions |
| **Audit** | Structured JSON logs, PII redacted, immutable events for critical actions |
| **Rate Limiting** | Per endpoint, IP, and account; captcha for suspicious flows |
| **Validation** | Zod for all inputs and public responses |
| **Dependencies** | SCA in CI, locked versions |

## Integration with r365-wallet

- `r365-core` delegates balance, fees, and transaction operations to `r365-wallet`
- Cross-service calls must sign/validate sensitive payloads
- Maintain contract tests and mocks in staging

## Reference Docs

- `.github/project-context.md` — Full business requirements
- `.github/result-pattern-services.md` — Result Pattern deep-dive
- `docs/http-provider.md` — HTTP client patterns
- `docs/keycloak-architecture.md` — Auth provider details
