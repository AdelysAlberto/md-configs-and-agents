# 🏛️ SOLID Principles & Clean Architecture Guide

## 🎯 Architecture Principles Applied

This project strictly follows SOLID principles and Clean Architecture patterns with a functional programming approach.

## 🔧 SOLID Principles Implementation

### 1. **Single Responsibility Principle (SRP)**

Each module has a single, well-defined responsibility:

```typescript
// ✅ CORRECT: Each function has one responsibility
export const validateEmailService = async (email: string, code: string) => {
  // Only validates email codes
};

export const activateUserService = async (userId: string) => {
  // Only activates users
};

// ❌ WRONG: Multiple responsibilities in one function
export const validateAndActivateService = async (email: string, code: string) => {
  // Validates AND activates - violates SRP
};
```

### 2. **Open/Closed Principle (OCP)**

Code is open for extension, closed for modification:

```typescript
// ✅ CORRECT: Extensible through composition
type ValidationRule = (value: string) => boolean;

const validateInput = (value: string, rules: ValidationRule[]) => {
  return rules.every(rule => rule(value));
};

// New validation rules can be added without modifying existing code
const emailRules: ValidationRule[] = [
  isNotEmpty,
  isValidEmail,
  hasValidDomain,
];
```

### 3. **Liskov Substitution Principle (LSP)**

Functions can be replaced with more specific implementations:

```typescript
// ✅ CORRECT: Repository abstractions are substitutable
type UserRepository = {
  findById: (id: string) => Promise<User | null>;
  create: (data: CreateUserData) => Promise<User>;
};

// Keycloak implementation
export const keycloakUserRepository: UserRepository = {
  findById: (id) => keycloakProvider.findById(id),
  create: (data) => keycloakProvider.create(data),
};

// Database implementation (future)
export const databaseUserRepository: UserRepository = {
  findById: (id) => database.findById(id),
  create: (data) => database.create(data),
};
```

### 4. **Interface Segregation Principle (ISP)**

Clients depend only on interfaces they use:

```typescript
// ✅ CORRECT: Segregated interfaces
type UserReader = {
  findById: (id: string) => Promise<User | null>;
  findByEmail: (email: string) => Promise<User | null>;
};

type UserWriter = {
  create: (data: CreateUserData) => Promise<User>;
  update: (id: string, data: UpdateUserData) => Promise<User>;
};

// Services only depend on what they need
export const getUserService = (userReader: UserReader) => 
  async (id: string) => userReader.findById(id);
```

### 5. **Dependency Inversion Principle (DIP)**

High-level modules don't depend on low-level modules:

```typescript
// ✅ CORRECT: Service depends on abstraction, not implementation
export const createUserService = async (userData: CreateUserInput) => {
  // Depends on Repository interface, not Keycloak directly
  const result = await UserRepository.createUser(userData);
  return result;
};

// ❌ WRONG: Direct dependency on implementation
export const badCreateUserService = async (userData: CreateUserInput) => {
  // Direct dependency on Keycloak
  const result = await keycloakClient.users.create(userData);
  return result;
};
```

## 🏗️ Clean Architecture Layers

### Layer 1: **Entities (Domain Types)**
```typescript
// Pure business objects with no dependencies
type User = {
  id: string;
  email: string;
  username: string;
  isActive: boolean;
  emailVerified: boolean;
};

type CreateUserInput = {
  email: string;
  username: string;
  password: string;
};
```

### Layer 2: **Use Cases (Services)**
```typescript
// Business logic orchestration
export const createUserService = async (input: CreateUserInput) => {
  // Business rules and validation
  if (!isValidEmail(input.email)) {
    throw AppError.BadRequest("InvalidEmailFormat");
  }

  // Orchestrate repository calls
  const existingUser = await UserRepository.findByEmail(input.email);
  if (existingUser) {
    throw AppError.Conflict("UserAlreadyExists");
  }

  return await UserRepository.createUser(input);
};
```

### Layer 3: **Interface Adapters (Controllers & Repositories)**
```typescript
// Controllers: HTTP to domain translation
export const createUserController = async (req: Request, res: Response): Promise<void> => {
  const result = await createUserService(req.body);
  res.status(HttpStatus.CREATED).json(result);
};

// Repositories: Domain to external service translation
export async function createUser(data: CreateUserInput) {
  return keycloakProvider.create({
    // Map domain model to external service format
    username: data.username,
    email: data.email,
    // ... provider-specific format
  });
}
```

### Layer 4: **Frameworks & Drivers (Providers)**
```typescript
// External service integration
export const keycloakProvider = {
  create: async (userData: KeycloakUserData) => {
    // Direct Keycloak API calls
    return await kc.users.create(userData);
  },
};
```

## 🎯 Functional Programming Principles

### 1. **Pure Functions**
```typescript
// ✅ CORRECT: Pure function
const calculateUserAge = (birthDate: Date): number => {
  const today = new Date();
  return today.getFullYear() - birthDate.getFullYear();
};

// ❌ WRONG: Impure function (side effects)
let userCount = 0;
const badCreateUser = (userData: CreateUserInput) => {
  userCount++; // Side effect
  console.log("Creating user"); // Side effect
  return userData;
};
```

### 2. **Immutability**
```typescript
// ✅ CORRECT: Immutable updates
const updateUser = (user: User, updates: Partial<User>): User => {
  return { ...user, ...updates };
};

// ❌ WRONG: Mutation
const badUpdateUser = (user: User, updates: Partial<User>): User => {
  user.email = updates.email; // Mutates original object
  return user;
};
```

### 3. **Function Composition**
```typescript
// ✅ CORRECT: Composable functions
const validateEmail = (email: string): boolean => isValidEmail(email);
const checkEmailExists = async (email: string): Promise<boolean> => {
  const user = await UserRepository.findByEmail(email);
  return !!user;
};

const validateNewUser = async (userData: CreateUserInput): Promise<boolean> => {
  return validateEmail(userData.email) && 
         !(await checkEmailExists(userData.email));
};
```

## 🔒 Type Safety Guidelines

### 1. **No Any Types**
```typescript
// ✅ CORRECT: Explicit types
type APIResponse<T> = {
  success: boolean;
  data?: T;
  error?: string;
};

// ❌ WRONG: Using any
const badAPICall = (data: any): any => {
  return processData(data);
};
```

### 2. **Union Types Over Enums**
```typescript
// ✅ CORRECT: Union types
type UserStatus = "active" | "inactive" | "pending";

// Less preferred: enums
enum UserStatusEnum {
  ACTIVE = "active",
  INACTIVE = "inactive", 
  PENDING = "pending"
}
```

### 3. **Type Guards**
```typescript
// ✅ CORRECT: Type guards for unknown data
const isUser = (data: unknown): data is User => {
  return typeof data === "object" && 
         data !== null && 
         "id" in data && 
         "email" in data;
};

const processUserData = (data: unknown) => {
  if (isUser(data)) {
    // TypeScript knows data is User here
    console.log(data.email);
  }
};
```

## 📋 Architecture Checklist

### ✅ Clean Architecture Compliance
- [ ] Dependencies point inward (toward domain)
- [ ] External concerns isolated in outer layers
- [ ] Business logic independent of frameworks
- [ ] Domain types have no external dependencies

### ✅ SOLID Compliance
- [ ] Each function has single responsibility
- [ ] Code is extensible without modification
- [ ] Abstractions are substitutable
- [ ] Interfaces are client-specific
- [ ] Dependencies are inverted

### ✅ Functional Programming
- [ ] No classes (except utilities like AppError)
- [ ] Pure functions without side effects
- [ ] Immutable data structures
- [ ] Function composition over inheritance

### ✅ Type Safety
- [ ] No `any` types used
- [ ] Explicit return types
- [ ] Type guards for unknown data
- [ ] Union types for constrained values

## 🎭 Anti-Patterns to Avoid

### ❌ God Services
```typescript
// WRONG: One service doing everything
export const userService = {
  create: () => {},
  validate: () => {},
  sendEmail: () => {},
  updatePassword: () => {},
  generateReports: () => {},
  // ... 20 more methods
};
```

### ❌ Leaky Abstractions
```typescript
// WRONG: Keycloak details leaking into service
export const createUserService = async (userData: CreateUserInput) => {
  // Service shouldn't know about Keycloak UserRepresentation
  const keycloakUser: UserRepresentation = {
    username: userData.username,
    attributes: { customField: ["value"] } // Keycloak-specific
  };
};
```

### ❌ Mixed Concerns
```typescript
// WRONG: HTTP and business logic mixed
export const badController = async (req: Request, res: Response) => {
  // Business logic in controller
  if (req.body.age < 18) {
    throw new Error("Too young");
  }
  
  // Database logic in controller  
  const user = await db.users.create(req.body);
  
  res.json(user);
};
```

---

> **Remember**: Architecture is about making the right things easy and the wrong things hard. These principles guide us toward maintainable, testable, and scalable code.
