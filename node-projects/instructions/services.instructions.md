---
applyTo: "**/services/**"
---

# Services — Business Logic with Result Pattern

## Core Rules

- Services contain ALL business logic
- Return `Result<T>` using `ResponseOk(data)` / `ResponseFail.X("ErrorCode")`
- **NEVER throw** — use Result Pattern exclusively
- `try/catch` ONLY for external provider calls (Keycloak, HTTP APIs)
- Each service function has a **single responsibility**

## Service Template

```typescript
import { ResponseOk, ResponseFail } from "@/utils/result";
import type { Result } from "@/utils/result";
import { logger } from "@/provider/logger";

export const {action}Service = async (
  param1: string,
  param2: string
): Promise<Result<EntityType>> => {
  logger.info("[{action}Service]: Starting operation");

  if (!isValid(param1)) {
    logger.warn("[{action}Service]: Invalid param1 format");
    return ResponseFail.BadRequest("InvalidParam1Format");
  }

  const data = await Repository.findByCriteria(param1);
  if (!data) {
    logger.warn("[{action}Service]: Data not found");
    return ResponseFail.NotFound("DataNotFound");
  }

  const result = await Repository.updateEntity(data.id, { param2 });
  if (!result.success) {
    logger.error("[{action}Service]: Failed to update entity");
    return ResponseFail.UnprocessableEntity("FailedToUpdateEntity");
  }

  logger.info("[{action}Service]: ✅ Operation completed successfully");
  return ResponseOk(result);
};
```

## ResponseFail Error Mapping

| Method | HTTP Status | Use Case |
|--------|------------|----------|
| `ResponseFail.BadRequest()` | 400 | Invalid input |
| `ResponseFail.Unauthorized()` | 401 | Invalid credentials |
| `ResponseFail.Forbidden()` | 403 | Access denied |
| `ResponseFail.NotFound()` | 404 | Resource not found |
| `ResponseFail.Conflict()` | 409 | Duplicate resource |
| `ResponseFail.UnprocessableEntity()` | 422 | Business rule violation |
| `ResponseFail.Internal()` | 500 | Internal error |

## Paginated Service Pattern

```typescript
export const getItemsService = async (
  userId: string, limit: number, offset: number
): Promise<Result<{ items: Item[]; total: number }>> => {
  if (!userId) return ResponseFail.BadRequest("UserIdRequired");

  const items = await Repository.getItems(userId, limit, offset);
  const total = await Repository.countItems(userId);

  return ResponseOk({ items, total });
};
```

## External Provider Calls (try/catch allowed)

```typescript
export const changePasswordService = async (
  userId: string, newPassword: string
): Promise<Result<{ success: boolean }>> => {
  // ... validations with ResponseFail...

  try {
    await externalProvider.updatePassword(userId, newPassword);
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

## Service-to-Service Composition

When consuming another service, propagate failures with `return`:

```typescript
const userResult = await getUserBasicInfoService(contact.userId);
if (!userResult.success) return userResult; // Propagate failure

return ResponseOk({ contact, userDetails: userResult.data });
```

## Promise.all with Graceful Skipping

```typescript
const promises = items.map(async (item) => {
  const result = await getInfoService(item.id);
  if (!result.success) {
    logger.warn(`Skipping item ${item.id}`);
    return null;
  }
  return { id: item.id, info: result.data };
});

const results = await Promise.all(promises);
const valid = results.filter((r): r is NonNullable<typeof r> => r !== null);
```

## ❌ Forbidden in Services

```typescript
// ❌ throw AppError.X() — use ResponseFail.X()
// ❌ God services (many unrelated methods in one object)
// ❌ Direct imports of repositories from OTHER domains
// ❌ HTTP/Express concerns (req, res)
```
