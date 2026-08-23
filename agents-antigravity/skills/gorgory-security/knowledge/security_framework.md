# Pragmatic Security & App Protection Framework - Chief Wiggum

This document details technical security guidelines and protection standards evaluated and enforced by **Chief Wiggum (Jefe Gorgory)**.

---

## 1. Pragmatic Security Principles (Zero Bureaucracy)

1. **Simplicity First (DRY / YAGNI)**: Do NOT introduce cumbersome layers or unnecessary proxies if a simple server rule handles the threat cleanly.
2. **Backend Perimeter Protection**:
   - **Rate Limiting**: Enforce request rate limits on sensitive endpoints (e.g., `/api/auth/login`, `/api/auth/register`, `/api/auth/reset-password`).
   - **OWASP Sanitization**: Validate and clean all incoming payloads to prevent XSS, SQL Injection, and CSRF.
   - **Authentication Tokens**: Enforce `HttpOnly` and `SameSite` flags for session cookies or short-lived tokens.
   - **Structured Logging**: Log security events without leaking stack traces or sensitive credentials to clients.
3. **Frontend Shielding & Performance**:
   - **Zero Secret Exposure**: Never store private keys or sensitive credentials in `localStorage` or client state.
   - **Bottleneck Elimination**: Audit and remove unnecessary re-renders, duplicate requests, and memory leaks.
   - **Route Protection**: Enforce secure client-side route guards based on authentication state.

---

## 2. Deliverables & Output Artifacts

- `artifacts/security_specification.md`: Security technical specifications, rate-limiting policies, OWASP sanitization rules, and frontend performance shielding guidelines.

