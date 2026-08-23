---
applyTo: "**/controllers/**, **/routes/**"
---

# Controllers — Thin HTTP Adapters

## Core Rules

- Controllers are **thin HTTP adapters** — NO business logic
- **NO try/catch** — errorHandler middleware is the safety net
- Delegate ALL logic to services
- Two patterns: **simple** (sendResult) and **paginated** (manual response)

## Simple Controller Pattern (sendResult)

```typescript
import { sendResult } from "@/utils/result";
import { HttpStatus } from "@/interfaces";
import { logger } from "@/provider/logger";

export const getContactDetailsController = async (req: Request, res: Response): Promise<void> => {
  const { contactId } = req.params;

  if (!contactId) {
    logger.warn("[getContactDetailsController]: Missing contactId");
    res.status(HttpStatus.BAD_REQUEST).json({ error: "ContactIdRequired" });
    return;
  }

  const result = await getContactDetailsService(contactId);

  sendResult(res, result, "Contact details retrieved successfully");
};
```

## Paginated Controller Pattern

```typescript
import { createPaginationMeta, createSuccessResponse } from "@/interfaces/api-response";

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

  // ✅ Destructure to extract the ARRAY directly
  const { total, contacts } = result.data;
  const pagination = createPaginationMeta(total, Number(limit), Number(offset));

  // ✅ data = contacts (the array), NOT { contacts: [...] }
  res.status(HttpStatus.OK).json(
    createSuccessResponse("Contacts retrieved successfully", contacts, pagination)
  );
};
```

## Inline Validation

Controllers validate **presence** of required fields only (not business rules):

```typescript
if (!param1 || !param2) {
  logger.warn("[controllerName]: Missing required fields");
  res.status(HttpStatus.BAD_REQUEST).json({ error: "Param1AndParam2Required" });
  return;
}
```

## ❌ Forbidden in Controllers

```typescript
// ❌ Business logic
if (user.isActive && user.emailVerified) { /* ... */ }

// ❌ try/catch
try { await service(); } catch (e) { /* ... */ }

// ❌ Database access
const user = await db.users.create(req.body);

// ❌ status field in response body
res.json({ data: result, status: 200 });
```
