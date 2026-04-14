# 🏦 r365-core — Project Context & Technical Guidance

Este documento es la fuente de referencia para el servicio `r365-core` (usuarios, perfiles, contactos). Contiene el alcance funcional, principios arquitecturales, contratos API clave y prácticas de seguridad FinTech. Las operaciones de ledger, saldos y movimientos pertenecen a `r365-wallet` y siempre se delegan a ese servicio.

## 📋 Resumen y Alcance
- Servicio: `r365-core`
- Responsabilidades: gestión de cuentas, perfiles, contactos, orquestación KYC, autenticación, auditoría y eventos.
- Exclusiones: no implementar lógica de saldo, cálculo de fees ni movimientos; delegar a `r365-wallet`.

## 🎯 Prioridades
1. Seguridad de cuentas y autenticación (tokens, refresh rotation, MFA)
2. Flujos robustos de contactos (request, accept, reject, block) con eventos auditable
3. Validación estricta de inputs y contract tests (OpenAPI)
4. Observabilidad y auditoría inmutable para acciones críticas

## 🏗️ Principios arquitecturales
- Clean Architecture: `controllers` delgados, `services` con lógica de negocio, `repositories` para acceso a datos.
- Result Pattern: los servicios devuelven `ResponseOk` / `ResponseFail`; evitar throws para errores de negocio.
- Respetar límites de dominio: no importar repositorios de otros servicios — usar APIs o mensajería.
- Tipado estricto en TypeScript y validación con `zod`.

---

## 🔌 API surface (resumen)
Mantener OpenAPI en `src/docs` y actualizar contratos con cada cambio.

- Autenticación
  - `POST /auth/login` — devuelve `access` + `refresh` tokens
  - `POST /auth/refresh` — refresh rotation
  - `POST /auth/logout` — revocación de refresh token

- Usuarios
  - `POST /users` — registrar (email, password, nombre). Verificación por email asíncrona
  - `GET /users/:id` — perfil público (campos minimizados)
  - `PATCH /users/:id` — actualizar (autorizado)

- Contactos
  - `POST /contacts` — enviar solicitud
  - `POST /contacts/:id/accept` — aceptar
  - `POST /contacts/:id/reject` — rechazar
  - `GET /contacts` — listar (paginado)

Todos los endpoints deben devolver: `{ message, data, pagination? }`. Errores: `{ statusCode, error }`.

---

## 📐 Modelos clave (resumen)
- `User` — `id`, `email` (normalizado), `emailVerified`, `passwordHash` (argon2), `firstName`, `lastName`, `kycStatus`, `createdAt`, `updatedAt`
- `Contact` — `id`, `ownerId`, `contactUserId`, `status` (`pending|accepted|rejected|blocked`), `alias?`, `isFavorite?`, `createdAt`

No almacenar contraseñas en texto claro ni secrets en repositorios.

---

## 🔐 Seguridad — prácticas obligatorias (FinTech)

1. Autenticación y autorización
   - Access tokens con TTL corto; refresh tokens rotativos y revocables.
   - Persistir sólo identificadores (hash) de refresh tokens para permitir rotación/revocación.
   - Soportar MFA (TOTP) para operaciones sensibles.

2. Gestión de contraseñas
   - Hash con `argon2id` (recomendado) o `bcrypt` con parámetros fuertes.
   - Políticas de complejidad y throttling en intentos fallidos.

3. Encriptación
   - TLS 1.2+/1.3 para todo el tráfico.
   - Datos sensibles en reposo cifrados (KMS / field-level encryption).

4. Secrets & Keys
   - Usar gestor de secretos (Vault / AWS KMS / Azure KeyVault). No versionar secretos.
   - Rotación periódica y acceso auditado.

5. Principle of Least Privilege
   - Usuarios DB y credenciales con permisos mínimos necesarios.

6. Logging & Auditing
   - Logs estructurados (JSON); redactar PII; eventos de auditoría inmutables para acciones críticas (cambios de email, KYC, creación/aceptación de contactos).

7. Rate limiting & Abuse protection
   - Límites por endpoint, IP y por cuenta; captchas/backoff en flujos sospechosos.

8. Fraud detection
   - Señales: geolocation inconsistent, IP churn, device fingerprinting, patterns of rapid transactions.

9. Validation & Sanitization
   - Usar `zod`/`ajv` para todas las entradas y respuestas públicas.

10. Dependencies & SCA
   - Ejecutar análisis SCA en CI; mantener bloqueo de versiones en package manager.

11. Security testing
   - Fuzzing, SAST y pentesting periódicos.

---

## ⚙️ Operaciones, despliegue y observabilidad
- Contenedores reproducibles (Docker); `docker-compose` para desarrollo.
- Variables de entorno y secrets sólo desde gestor centralizado.
- CI pipelines: lint, unit tests, contract tests, SCA antes de deploy.
- Observabilidad: métricas (Prometheus), trazas (OpenTelemetry / Jaeger), alertas y dashboards.

---

## 🔗 Integración con `r365-wallet`
- `r365-core` delega operaciones que implican saldo, fees o movimientos a `r365-wallet`.
- Todas las integraciones cross-service deben firmar/validar payloads sensibles y almacenar evidencia para auditoría.
- Mantener contract tests (OpenAPI) y mocks en staging para validar integraciones.

---

## ✅ Checklist de seguridad previo a producción
- [ ] TLS configurado y verificado
- [ ] Keys y secrets en gestor seguro
- [ ] Refresh token rotation implementado
- [ ] Password hashing con parámetros fuertes
- [ ] Logging y auditoría activados y PII redacted
- [ ] Rate limits aplicados
- [ ] SCA y SAST en CI

---

## 📚 Referencias rápidas
- `/.github/TECH_DOC_CORE.md` — documentación técnica ampliada
- `/docs/http-provider.md` — patrones de cliente HTTP
- `/.github/copilot-instructions.md` — estándares de arquitectura y Result Pattern

Mantener este archivo como la fuente viva del scope y prácticas para `r365-core`.
