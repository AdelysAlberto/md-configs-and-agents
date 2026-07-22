# 🎯 Copilot Implementation Guide

## 📊 Analysis of Current Architecture

### Current Implementation Strengths

1. **Clean Separation of Concerns**
   - Controllers: Pure HTTP adapters without business logic
   - Services: Domain logic with proper error handling via AppError
   - Repositories: Clean data access abstraction over Keycloak provider

2. **Functional Programming Adherence**
   - No classes (except AppError utility)
   - Pure functions with explicit types
   - Immutable data patterns

3. **Error Handling Strategy**
   - Global error middleware catches all AppError instances
   - Controllers rely on middleware for error handling
   - Services throw domain-specific AppError types

4. **Type Safety**
   - Strict TypeScript configuration
   - No `any` types used
   - Explicit return types and input validation

## 🛠️ Step-by-Step Implementation Guide

### Step 1: Define Domain Types
```typescript
// Always start with clear type definitions
type CreateUserRequest = {
  username: string;
  email: string;
  password: string;
  firstName?: string;
  lastName?: string;
};

type UserResponse = {
  id: string;
  username: string;
  email: string;
  isActive: boolean;
  emailVerified: boolean;
};
```

### Step 2: Repository Layer Implementation
```typescript
// {domain}.repository.ts
export async function createUser(data: CreateUserRequest) {
  return keycloakProvider.create({
    username: data.username,
    email: data.email,
    enabled: true,
    emailVerified: false,
    firstName: data.firstName,
    lastName: data.lastName,
    credentials: [{
      type: "password",
      value: data.password,
      temporary: false,
    }],
    attributes: {
      isActive: ["true"],
      isEmailVerified: ["false"],
    },
  });
}

export async function findUserByEmail(email: string) {
  const results = await keycloakProvider.findBy({ email });
  return results.length > 0 ? results[0] : null;
}
```

### Step 3: Service Layer Implementation
```typescript
// {domain}.service.ts
export const createUserService = async (userData: CreateUserRequest) => {
  logger.info("[createUserService]: Starting user creation");

  // Input validation
  if (!isValidEmail(userData.email)) {
    logger.warn("[createUserService]: Invalid email format");
    throw AppError.BadRequest("InvalidEmailFormat");
  }

  if (!isValidPassword(userData.password)) {
    logger.warn("[createUserService]: Invalid password format");
    throw AppError.BadRequest("InvalidPasswordFormat");
  }

  // Business logic checks
  const existingUser = await Repository.findUserByEmail(userData.email);
  if (existingUser) {
    logger.warn("[createUserService]: User already exists");
    throw AppError.Conflict("UserAlreadyExists");
  }

  // Create user
  const result = await Repository.createUser(userData);
  if (!result.success) {
    logger.error("[createUserService]: Failed to create user");
    throw AppError.UnprocessableEntity("FailedToCreateUser");
  }

  logger.info("[createUserService]: ✅ User created successfully");
  return result;
};
```

### Step 4: Controller Implementation
```typescript
// {domain}.controller.ts
/**
 * Controller for user creation
 */
export const createUserController = async (req: Request, res: Response): Promise<void> => {
  const { username, email, password, firstName, lastName } = req.body;

  // Basic input validation (detailed validation in service)
  if (!username || !email || !password) {
    logger.warn("[createUserController]: Missing required fields");
    res.status(HttpStatus.BAD_REQUEST).json({
      message: "Username, email and password are required",
      status: HttpStatus.BAD_REQUEST,
    });
    return;
  }

  logger.info(`[createUserController]: Creating user - ${username}`);

  // Delegate to service (no try/catch needed)
  const result = await createUserService({
    username,
    email,
    password,
    firstName,
    lastName,
  });

  res.status(HttpStatus.CREATED).json({
    message: "User created successfully",
    data: result,
    status: HttpStatus.CREATED,
  });
};
```

### Step 5: Route Definition
```typescript
// {domain}.routes.ts
import { Router } from "express";
import { validate } from "@/middleware";
import { createUserSchema } from "./schemas/{domain}.schema";
import { createUserController } from "./controllers/{domain}.controller";

const router = Router();

router.post("/users", validate(createUserSchema), createUserController);

export { router as userRoutes };
```

## 🔄 Data Flow Pattern

```
Request → Route → Middleware → Controller → Service → Repository → Provider
                                    ↓
Response ← Controller ← Service ← Repository ← Provider
```

### Key Principles:
1. **Controllers**: HTTP concerns only (validation, response formatting)
2. **Services**: Business logic and domain rules
3. **Repositories**: Data access abstraction
4. **Providers**: External service integration (Keycloak, Database)

## 🎨 Common Patterns

### Pattern 1: Find with Fallback
```typescript
export const findUserService = async (identifier: string) => {
  logger.info("[findUserService]: Starting user search");

  const user = await Repository.findUserByEmail(identifier) ||
                await Repository.findUserByUsername(identifier);

  if (!user) {
    logger.warn("[findUserService]: User not found");
    throw AppError.NotFound("UserNotFound");
  }

  return user;
};
```

### Pattern 2: Update with Validation
```typescript
export const updateUserService = async (userId: string, updates: Partial<UserData>) => {
  logger.info("[updateUserService]: Starting user update");

  const user = await Repository.findUserById(userId);
  if (!user) {
    logger.warn("[updateUserService]: User not found");
    throw AppError.NotFound("UserNotFound");
  }

  if (updates.email && !isValidEmail(updates.email)) {
    logger.warn("[updateUserService]: Invalid email format");
    throw AppError.BadRequest("InvalidEmailFormat");
  }

  const result = await Repository.updateUser(userId, updates);
  if (!result.success) {
    logger.error("[updateUserService]: Failed to update user");
    throw AppError.UnprocessableEntity("FailedToUpdateUser");
  }

  logger.info("[updateUserService]: ✅ User updated successfully");
  return result;
};
```

### Pattern 3: List with Filters
```typescript
export const listUsersService = async (filters: UserFilters) => {
  logger.info("[listUsersService]: Starting user list retrieval");

  const users = await Repository.findUsers(filters);
  
  logger.info(`[listUsersService]: ✅ Retrieved ${users.length} users`);
  return users;
};
```

## 🚨 Error Mapping

### Service → AppError Mapping:
- Invalid input → `AppError.BadRequest`
- Resource not found → `AppError.NotFound`
- Business rule violation → `AppError.UnprocessableEntity`
- Duplicate resource → `AppError.Conflict`
- External service failure → `AppError.ServiceUnavailable`

### Controller Response Mapping:
- `AppError.BadRequest` → 400
- `AppError.NotFound` → 404
- `AppError.Conflict` → 409
- `AppError.UnprocessableEntity` → 422
- `AppError.ServiceUnavailable` → 503

## 📋 Development Checklist

When implementing new endpoints:

### ✅ Repository Layer
- [ ] Pure data access functions
- [ ] Provider abstraction maintained
- [ ] Clear input/output types
- [ ] No business logic

### ✅ Service Layer
- [ ] Input validation with AppError
- [ ] Business logic implementation
- [ ] Proper logging at key points
- [ ] Clean success/error paths

### ✅ Controller Layer
- [ ] Basic request validation
- [ ] Service delegation
- [ ] Response formatting
- [ ] NO try/catch blocks

### ✅ Type Safety
- [ ] No `any` types used
- [ ] Explicit return types
- [ ] Input type definitions
- [ ] Response type definitions

### ✅ Code Quality
- [ ] Function documentation in English
- [ ] Minimal inline comments
- [ ] Consistent naming patterns
- [ ] Functional programming principles

## 🎯 Quick Implementation Template

```typescript
// 1. Types
type {Feature}Input = {
  field1: string;
  field2?: string;
};

// 2. Repository
export async function {action}{Entity}(data: {Feature}Input) {
  return await provider.{action}(data);
}

// 3. Service
export const {action}{Entity}Service = async (input: {Feature}Input) => {
  logger.info("[{action}{Entity}Service]: Starting {action}");
  
  // Validation
  if (!isValid(input.field1)) {
    throw AppError.BadRequest("InvalidField1");
  }
  
  // Business logic
  const result = await Repository.{action}{Entity}(input);
  
  if (!result.success) {
    throw AppError.UnprocessableEntity("FailedTo{Action}{Entity}");
  }
  
  logger.info("[{action}{Entity}Service]: ✅ {Action} completed");
  return result;
};

// 4. Controller
export const {action}{Entity}Controller = async (req: Request, res: Response): Promise<void> => {
  const { field1, field2 } = req.body;
  
  if (!field1) {
    res.status(HttpStatus.BAD_REQUEST).json({
      message: "Field1 is required",
      status: HttpStatus.BAD_REQUEST,
    });
    return;
  }
  
  const result = await {action}{Entity}Service({ field1, field2 });
  
  res.status(HttpStatus.OK).json({
    message: "Success",
    data: result,
    status: HttpStatus.OK,
  });
};
```

---

> **Next Steps**: Use these patterns as templates for all new feature development. Maintain consistency across the codebase and always prioritize type safety and functional programming principles.
