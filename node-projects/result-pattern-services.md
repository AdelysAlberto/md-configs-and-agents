# Result Pattern — Node.js + Express

## Descripción

El **Result Pattern** reemplaza el uso de excepciones (`throw/catch`) por objetos que encapsulan éxito o fallo. Los servicios **nunca lanzan excepciones** a sus consumidores — retornan un `Result<T>`.

**Beneficios:**
- **Sin excepciones** — Control de flujo explícito, sin sorpresas en runtime
- **Tipado fuerte** — TypeScript sabe exactamente qué retorna cada función
- **Funcional** — Las funciones retornan valores, no lanzan efectos secundarios
- **Performante** — Sin overhead de creación de stack traces por excepciones
- **Simple** — `ResponseOk(data)` para éxito, `ResponseFail.X("code")` para error

---


## API del Result Pattern

### Constructores

```typescript
import { ResponseOk, ResponseFail } from "@/utils/result";

// Éxito
ResponseOk({ user: { id: "123", email: "john@test.com" } })

// Fallos tipados por HTTP status
ResponseFail.BadRequest("InvalidEmailFormat")       // 400
ResponseFail.Unauthorized("InvalidCredentials")     // 401
ResponseFail.Forbidden("AccessDenied")              // 403
ResponseFail.NotFound("UserNotFound")               // 404
ResponseFail.Conflict("AlreadyExists")              // 409
ResponseFail.UnprocessableEntity("FailedToCreate")  // 422
ResponseFail.Internal("InternalError")              // 500
```

### Helper de Express

```typescript
import { sendResult } from "@/utils/result";

// Envía la respuesta HTTP automáticamente según el Result
sendResult(res, result, "Operation successful");

// Con opciones
sendResult(res, result, "Created", { statusCode: 201 });
sendResult(res, result, "List retrieved", { pagination });
```

### Type Guards

```typescript
import { isSuccess, isFailure } from "@/utils/result";

if (isSuccess(result)) {
  // result.data disponible
}

if (isFailure(result)) {
  // result.error y result.statusCode disponibles
}
```

---

## Patrón en Servicios

Los servicios retornan `Promise<Result<T>>`. Nunca hacen `throw`.

```typescript
import { ok, Fail } from "@/utils/result";
import type { Result } from "@/utils/result";
import { logger } from "@/provider/logger";

type LoginData = {
  accessToken: string;
  refreshToken: string;
  user: { id: string; email: string };
};

export const loginService = async (username: string, password: string): Promise<Result<LoginData>> => {
  logger.info("[loginService]: Starting login process");

  if (!username || !password) {
    logger.warn("[loginService]: Missing credentials");
    return ResponseFail.BadRequest("UsernameAndPasswordRequired");
  }

  const user = await UserRepository.findByEmail(username);
  if (!user) {
    logger.warn("[loginService]: User not found");
    return ResponseFail.NotFound("UserNotFound");
  }

  if (!user.isActive) {
    logger.warn("[loginService]: User not active");
    return ResponseFail.UnprocessableEntity("UserNotActive");
  }

  const auth = await AuthRepository.validateCredentials(username, password);
  if (!auth.success) {
    logger.warn("[loginService]: Invalid credentials");
    return ResponseFail.Unauthorized("InvalidCredentials");
  }

  logger.info("[loginService]: ✅ Login successful");
  return ResponseOk({
    accessToken: auth.accessToken,
    refreshToken: auth.refreshToken,
    user: { id: user.id, email: user.email },
  });
};
```

---

## Patrón en Controllers

Los controllers usan `sendResult` para enviar la respuesta. Mantienen validaciones inline para campos requeridos.

```typescript
import type { Request, Response } from "express";
import { HttpStatus } from "@/interfaces";
import { sendResult } from "@/utils/result";
import { logger } from "@/provider/logger";
import { loginService } from "../services/auth.service";

export const loginController = async (req: Request, res: Response): Promise<void> => {
  const { username, password } = req.body;

  if (!username || !password) {
    logger.warn("[loginController]: Missing credentials");
    res.status(HttpStatus.BAD_REQUEST).json({ error: "UsernameAndPasswordRequired" });
    return;
  }

  logger.info(`[loginController]: Login attempt for - ${username}`);

  const result = await loginService(username, password);

  sendResult(res, result, "Login successful");
};
```

---

## Comunicación entre Servicios

Cuando un servicio consume otro servicio, propaga el fallo con `return`:

```typescript
export const getContactDetailsService = async (
  userId: string,
  contactId: string
): Promise<Result<ContactDetails>> => {
  logger.info("[getContactDetailsService]: Getting contact details");

  const contact = await ContactRepository.findRelationship(userId, contactId);
  if (!contact) return ResponseFail.NotFound("ContactNotFound");

  // Consumir otro servicio — propagar fallo si ocurre
  const userResult = await getUserBasicInfoService(contact.contactUserId);
  if (!userResult.success) return userResult;

  return ResponseOk({
    id: contact.id,
    contactUserId: contact.contactUserId,
    contactInfo: userResult.data,
  });
};
```

**Regla:** Si el sub-servicio falla, retorna su fallo directamente (`return userResult`). TypeScript lo permite porque `Failure` es parte de cualquier `Result<T>`.

---

## Llamadas HTTP Externas

Para proveedores externos que pueden lanzar excepciones (Keycloak, APIs externas), usa `try/catch` **internamente** en el servicio. El servicio sigue retornando `Result<T>` a su consumidor.

```typescript
export const changePasswordService = async (
  userId: string,
  currentPassword: string,
  newPassword: string
): Promise<Result<{ success: boolean }>> => {
  // ... validaciones con ResponseFail.X() ...

  try {
    await keycloakHttpClient.postForm({ url: tokenUrl, formData });

    const updateResult = await UserRepository.updatePassword(userId, newPassword);
    if (!updateResult.success) {
      return ResponseFail.UnprocessableEntity("FailedToUpdatePassword");
    }

    return ResponseOk({ success: true });
  } catch (error) {
    if (error && typeof error === "object" && "status" in error) {
      const httpError = error as IHttpError;
      if (httpError.status === 401) {
        return ResponseFail.Unauthorized("CurrentPasswordIncorrect");
      }
    }
    return ResponseFail.UnprocessableEntity("FailedToChangePassword");
  }
};
```

**Regla:** `try/catch` solo para proveedores externos. El servicio siempre retorna `Result<T>`, nunca lanza.

---

## Listas con Paginación

```typescript
// Servicio
export const getContactsService = async (
  userId: string,
  limit: number,
  offset: number
): Promise<Result<{ contacts: Contact[]; total: number }>> => {
  if (!userId) return ResponseFail.BadRequest("UserIdRequired");

  const contacts = await ContactRepository.getContacts(userId, limit, offset);
  const total = await ContactRepository.countContacts(userId);

  return ResponseOk({ contacts, total });
};

// Controller
export const getContactsController = async (req: Request, res: Response): Promise<void> => {
  const { limit = 20, offset = 0 } = req.query;
  const userId = req.user?.userId;

  if (!userId) {
    res.status(HttpStatus.UNAUTHORIZED).json({ error: "Unauthorized" });
    return;
  }

  const result = await getContactsService(userId, Number(limit), Number(offset));

  if (!result.success) {
    res.status(result.statusCode).json({ error: result.error });
    return;
  }

  const pagination = createPaginationMeta(result.data.total, Number(limit), Number(offset));

  sendResult(res, result, "Contacts retrieved successfully", { pagination });
};
```

---

## Manejo de Items con Promise.all

Cuando iteras sobre items que pueden fallar individualmente (ej: obtener info de usuarios):

```typescript
const contactPromises = relations.map(async (relation) => {
  const userResult = await getUserBasicInfoService(relation.contactUserId);

  // Si falla, skip — no propagar el error
  if (!userResult.success) {
    logger.warn(`Skipping non-existent user ${relation.contactUserId}`);
    return null;
  }

  return {
    id: relation.id,
    contactInfo: userResult.data,
  };
});

const results = await Promise.all(contactPromises);
const contacts = results.filter((c): c is NonNullable<typeof c> => c !== null);
```

---

## ErrorHandler como Safety Net

El `errorHandler` middleware se mantiene como red de seguridad para errores inesperados (bugs, errores de programación). Con el Result Pattern, no debería activarse en flujo normal.

```typescript
// src/middleware/errorHandler.middleware.ts
export const errorHandler = (err: Error, req: Request, res: Response, _next: NextFunction) => {
  logger.error(`[errorHandler][500]: Unexpected error - ${req.method}-${req.url}-[${err.message}]`);
  return res.status(500).json({ error: "internalServerError" });
};
```

---

## Migración: AppError (throw) → Fail (return)

### Paso 1: Servicio

**Antes (usando AppError):**
```typescript
import { AppError } from "@/utils/appError";

export const getUserService = async (id: string) => {
  if (!id) throw AppError.BadRequest("UserIdRequired");

  const user = await Repository.findById(id);
  if (!user) throw AppError.NotFound("UserNotFound");

  return { data: user };
};
```

**Después (usando Fail + Result):**
```typescript
import { ok, Fail } from "@/utils/result";
import type { Result } from "@/utils/result";

export const getUserService = async (id: string): Promise<Result<User>> => {
  if (!id) return ResponseFail.BadRequest("UserIdRequired");

  const user = await Repository.findById(id);
  if (!user) return ResponseFail.NotFound("UserNotFound");

  return ResponseOk(user);
};
```

**Cambios clave:**
- `throw AppError.X()` → `return ResponseFail.X()`
- Sin wrapper `{ data: ... }` en retorno
- Tipo de retorno explícito: `Promise<Result<T>>`
- Importar desde `@/utils/result`, no `@/utils/appError`

### Paso 2: Controller

**Antes:**
```typescript
const result = await getUserService(id);
res.status(HttpStatus.OK).json(createSuccessResponse("User retrieved", result.data));
```

**Después:**
```typescript
const result = await getUserService(id);
sendResult(res, result, "User retrieved");
```

---

## Checklist

Al crear o migrar un servicio:

- [ ] Importa `ok`, `Fail` desde `@/utils/result`
- [ ] El tipo de retorno es `Promise<Result<T>>`
- [ ] **NO usa `throw`** — retorna `ResponseFail.X("ErrorCode")`
- [ ] **NO usa `try/catch`** excepto para proveedores externos
- [ ] Éxito retorna `ResponseOk(data)` — sin wrapper `{ data: ... }`
- [ ] Fallo propaga con `return failResult` entre servicios

Al crear o migrar un controller:

- [ ] Importa `sendResult` desde `@/utils/result`
- [ ] Usa `sendResult(res, result, "message")` para respuestas
- [ ] Errores inline usan `{ error: "code" }` (consistente con errorHandler)
- [ ] **NO tiene `try/catch`**
- [ ] **NO tiene lógica de negocio**

---

## Formato de Respuestas

### Éxito (via `sendResult`)
```json
{
  "message": "User retrieved successfully",
  "data": { "id": "123", "email": "john@test.com" }
}
```

### Éxito con paginación
```json
{
  "message": "Contacts retrieved successfully",
  "data": { "contacts": [] },
  "pagination": { "total": 50, "limit": 20, "offset": 0, "hasMore": true }
}
```

### Error (via `sendResult` o inline)
```json
{
  "error": "UserNotFound"
}
```

---

## Comparación: AppError (throw) vs Result Pattern (return)

| Aspecto                | `throw AppError`                    | **Result Pattern (`Fail`)**         |
|------------------------|-------------------------------------|-------------------------------------|
| **Tipado**             | Error no está en el tipo de retorno | Error está en `Result<T>`          |
| **Control de flujo**   | Implícito (salta el stack)          | Explícito (return value)            |
| **Performance**        | Stack trace en cada throw           | Sin overhead                        |
| **Composición**        | Necesita try/catch para componer    | `if (!result.success) return result`|
| **Testing**            | Verificar excepciones               | Verificar `result.success`          |
| **Funcional**          | Efecto secundario (throw)           | Valor de retorno puro               |
| **Import**             | `import {AppError} from "@/utils/appError"` | `import {Fail, ok} from "@/utils/result"` |
| **Uso**                | `throw AppError.NotFound("msg")`    | `return ResponseFail.NotFound("msg")`       |

**Ejemplo lado a lado:**

```typescript
// ❌ ANTIGUO (AppError + throw)
export const oldGetUser = async (id: string) => {
  if (!id) throw AppError.BadRequest("IdRequired");
  const user = await Repository.findById(id);
  if (!user) throw AppError.NotFound("UserNotFound");
  return { data: user };
};

// ✅ NUEVO (Fail + return)
export const newGetUser = async (id: string): Promise<Result<User>> => {
  if (!id) return ResponseFail.BadRequest("IdRequired");
  const user = await Repository.findById(id);
  if (!user) return ResponseFail.NotFound("UserNotFound");
  return ResponseOk(user);
};
```

---

> **Regla:** Los servicios retornan `Result<T>` usando `ResponseFail.X()`. Nunca `throw AppError`. Los controllers usan `sendResult`. El `errorHandler` captura `AppError` solo como safety net para errores inesperados.
