---
applyTo: "src/modules/**"
---

# Domain-Driven Design — Inter-Domain Communication

## Core Rules

1. **Never import repositories from other domains**
2. **Always use exposed services** for inter-domain communication
3. Each domain **controls what it exposes**
4. Services can **compose** other domain services
5. Maintain **clear domain boundaries**

## ❌ WRONG: Direct Repository Access Across Domains

```typescript
// contacts.service.ts — NEVER DO THIS
import { UserRepository } from "@/modules/user/repositories/user.repository";

export const getContactService = async (contactId: string) => {
  const contact = await ContactRepository.findById(contactId);
  const userDetails = await UserRepository.findById(contact.userId); // ❌ FORBIDDEN
  return { contact, userDetails };
};
```

## ✅ CORRECT: Service-to-Service Communication

```typescript
// user.service.ts — Expose basic info for other domains
export const getUserBasicInfoService = async (
  userId: string
): Promise<Result<UserBasicInfo>> => {
  const user = await UserRepository.findById(userId);
  if (!user) return ResponseFail.NotFound("UserNotFound");

  return ResponseOk({
    id: user.id,
    email: user.email,
    firstName: user.firstName,
    lastName: user.lastName,
  });
};

// contacts.service.ts — Consume via service
import { getUserBasicInfoService } from "@/modules/user/services/user.service";

export const getContactService = async (
  contactId: string
): Promise<Result<ContactDetails>> => {
  const contact = await ContactRepository.findById(contactId);
  if (!contact) return ResponseFail.NotFound("ContactNotFound");

  const userResult = await getUserBasicInfoService(contact.userId);
  if (!userResult.success) return userResult; // Propagate failure

  return ResponseOk({ contact, userDetails: userResult.data });
};
```

## Domain Service Types

### 1. Basic Info Services
Each domain exposes read-only info for consumption by other domains.

### 2. Validation Services
Domain-specific validations (e.g., `validateUserExistsService`, `validateContactRelationshipService`).

### 3. Cross-Domain Operations
Compose multiple domain services for complex operations:

```typescript
export const transferService = async (fromUserId: string, toContactId: string, amount: number): Promise<Result<TransferData>> => {
  const userResult = await validateUserExistsService(fromUserId);
  if (!userResult.success) return userResult;

  const relationResult = await validateContactRelationshipService(fromUserId, toContactId);
  if (!relationResult.success) return relationResult;

  // ... business logic
};
```

## Benefits

- **Encapsulation**: Each domain controls its data access
- **Integrity**: Domain rules always enforced
- **Maintainability**: Changes in one domain don't break others
- **Testability**: Easy to mock inter-domain dependencies
