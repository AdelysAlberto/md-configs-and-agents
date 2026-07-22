# 🧠 Copilot Development Patterns & Guidelines

## 📋 Architecture Analysis Summary

Based on the existing codebase analysis, this document establishes the mandatory patterns and practices for developing new features in the User Management Service.

## 🏗️ Architectural Patterns Identified

### 1. **Clean Architecture Layers**
- **Controllers**: Thin HTTP adapters, NO business logic
- **Services**: Pure business logic with AppError handling
- **Repositories**: Data access layer abstraction
- **Providers**: External service integrations (Keycloak, Email)

### 2. **Error Handling Strategy**
- Controllers: **NO try/catch blocks** - let middleware handle errors
- Services: Use `AppError` static methods for domain-specific errors
- Global error handling via `errorHandler.middleware.ts`

### 3. **Functional Programming Enforcement**
- **NO classes** except for error handling (`AppError`)
- **NO constructors** or object-oriented patterns
- Pure functions with explicit return types
- Immutable data structures

## 🎯 Mandatory Development Patterns

### 📁 File Structure Pattern
```
src/modules/{domain}/
├── controllers/
│   └── {domain}.controller.ts
├── services/
│   └── {domain}.service.ts
├── repositories/
│   └── {domain}.repository.ts
├── schemas/
│   └── {domain}.schema.ts
└── routes/
    └── {domain}.routes.ts
```

### 🎛️ Controller Pattern (MANDATORY)
```typescript
/**
 * Controller for {action} operation
 */
export const {action}Controller = async (req: Request, res: Response): Promise<void> => {
  const { param1, param2 } = req.body;

  if (!param1 || !param2) {
    logger.warn("[{action}Controller]: Missing required fields");
    res.status(HttpStatus.BAD_REQUEST).json({
      message: "Param1 and param2 are required",
      status: HttpStatus.BAD_REQUEST,
    });
    return;
  }

  logger.info(`[{action}Controller]: Processing {action} request`);

  const result = await {action}Service(param1, param2);

  res.status(HttpStatus.OK).json({
    message: "Operation successful",
    data: result,
    status: HttpStatus.OK,
  });
};
```

### 🧠 Service Pattern (MANDATORY)
```typescript
export const {action}Service = async (param1: string, param2: string) => {
  logger.info("[{action}Service]: Starting {action} service");

  if (!isValid(param1)) {
    logger.warn("[{action}Service]: Invalid param1 format");
    throw AppError.BadRequest("InvalidParam1Format");
  }

  const data = await Repository.findBy{Criteria}(param1);
  if (!data) {
    logger.warn("[{action}Service]: Data not found");
    throw AppError.NotFound("DataNotFound");
  }

  const result = await Repository.update{Entity}(data.id, { param2 });

  if (!result.success) {
    logger.error("[{action}Service]: Failed to update entity");
    throw AppError.UnprocessableEntity("FailedToUpdateEntity");
  }

  logger.info("[{action}Service]: ✅ Operation completed successfully");
  return result;
};
```

### 🗃️ Repository Pattern (MANDATORY)
```typescript
export async function find{Entity}By{Criteria}(criteria: string) {
  const results = await provider.findBy({ criteria });
  return results.length > 0 ? results[0] : null;
}

export async function update{Entity}(id: string, data: Record<string, unknown>) {
  return await provider.update(id, data);
}

export async function create{Entity}(data: CreateEntityInput) {
  return provider.create({
    // Map domain data to provider format
    field1: data.field1,
    field2: data.field2,
    attributes: {
      customField1: [data.customField1],
      customField2: ["defaultValue"],
    },
  });
}
```

## 🚫 Forbidden Patterns

### ❌ Controllers with Business Logic
```typescript
// NEVER DO THIS
export const badController = async (req: Request, res: Response) => {
  try {
    // Business logic in controller - FORBIDDEN
    if (user.isActive && user.emailVerified) {
      // Complex validation logic
    }
  } catch (error) {
    // Try/catch in controller - FORBIDDEN
  }
};
```

### ❌ Services without AppError
```typescript
// NEVER DO THIS
export const badService = async (param: string) => {
  if (!param) {
    throw new Error("Invalid param"); // Use AppError instead
  }
};
```

### ❌ Any Type Usage
```typescript
// NEVER DO THIS
export const badFunction = (data: any) => { // NO ANY ALLOWED
  return data;
};
```

## ✅ Approved Patterns

### ✅ Service Error Handling
```typescript
export const goodService = async (email: string) => {
  logger.info("[goodService]: Starting service");

  if (!isValidEmail(email)) {
    logger.warn("[goodService]: Invalid email format");
    throw AppError.BadRequest("InvalidEmailFormat");
  }

  const user = await Repository.findUserByEmail(email);
  if (!user) {
    logger.warn("[goodService]: User not found");
    throw AppError.NotFound("UserNotFound");
  }

  return user;
};
```

### ✅ Controller Simplicity
```typescript
export const goodController = async (req: Request, res: Response): Promise<void> => {
  const { email } = req.body;

  if (!email) {
    logger.warn("[goodController]: Missing email");
    res.status(HttpStatus.BAD_REQUEST).json({
      message: "Email is required",
      status: HttpStatus.BAD_REQUEST,
    });
    return;
  }

  const result = await goodService(email);

  res.status(HttpStatus.OK).json({
    message: "Success",
    data: result,
    status: HttpStatus.OK,
  });
};
```

## 📝 Documentation Standards

### Function Documentation
```typescript
/**
 * Service for user email validation
 */
export const validateEmailService = async (email: string, code: string) => {
  // Implementation
};
```

### Type Definitions
```typescript
type CreateUserInput = {
  username: string;
  email: string;
  password: string;
  firstName?: string;
  lastName?: string;
};

type ServiceResult = {
  success: boolean;
  data?: unknown;
  error?: string;
};
```

## 🎯 Development Workflow

### When Creating New Features:

1. **Define Types First**
   ```typescript
   type {Feature}Input = {
     requiredField: string;
     optionalField?: string;
   };
   ```

2. **Create Repository Functions**
   ```typescript
   export async function create{Entity}(data: {Feature}Input) {
     return await provider.create(data);
   }
   ```

3. **Implement Service Logic**
   ```typescript
   export const {feature}Service = async (input: {Feature}Input) => {
     // Validation with AppError
     // Business logic
     // Repository calls
   };
   ```

4. **Create Thin Controllers**
   ```typescript
   export const {feature}Controller = async (req: Request, res: Response): Promise<void> => {
     // Input validation
     // Service call
     // Response formatting
   };
   ```

5. **Define Routes**
   ```typescript
   router.post("/{endpoint}", validate({feature}Schema), {feature}Controller);
   ```

## 🔒 Type Safety Rules

- **NEVER** use `any` type
- Use `unknown` for dynamic data with type guards
- Define explicit return types for all functions
- Use `Record<string, unknown>` for dynamic objects
- Prefer `type` over `interface` for data shapes

## 🧪 Testing Patterns

- Services: Unit tests with mocked repositories
- Controllers: Integration tests with request/response
- Repositories: Integration tests with real providers
- Use Vitest + Supertest for testing

## 📚 Code Examples Repository

Reference these existing implementations:
- `registration.controller.ts` - Clean controller patterns
- `registration.service.ts` - AppError usage and business logic
- `registration.repository.ts` - Provider abstraction
- `password.service.ts` - Validation and error handling

## 🚀 Quick Start Checklist

When implementing new features:
- [ ] Controllers have NO try/catch blocks
- [ ] Services use AppError for all exceptions
- [ ] NO `any` types used anywhere
- [ ] Functions have explicit return types
- [ ] Business logic ONLY in services
- [ ] Data access ONLY in repositories
- [ ] All functions are pure (no side effects except logging)
- [ ] Documentation in English
- [ ] Minimal comments (only JSDoc for functions)

---

> **Remember**: Controllers delegate, Services orchestrate, Repositories abstract. Keep it simple, functional, and type-safe.
