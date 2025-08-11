# 🧠 Copilot Instructions – Microservices Development Patterns

## 🎯 Overview

This document establishes the universal patterns and coding standards for building microservices with clean architecture, functional programming, and strict type safety. These patterns are technology-agnostic and should be followed across all microservice implementations.

## 📚 Reference Documents

For HTTP client usage: **[HTTP Provider Documentation](../docs/http-provider.md)** - **MANDATORY**: Centralized HTTP client usage guide

---

## 🏗️ Architectural Patterns

### **Clean Architecture Layers**
- **Controllers**: Thin HTTP adapters, NO business logic
- **Services**: Pure business logic with AppError handling  
- **Repositories**: Data access layer abstraction
- **Providers**: External service integrations

### **Error Handling Strategy**
- Controllers: **NO try/catch blocks** - let middleware handle errors
- Services: Use `AppError` static methods for domain-specific errors
- Global error handling via `errorHandler.middleware.ts`

### **Functional Programming Enforcement**
- **NO classes** except for error handling (`AppError`)
- **NO constructors** or object-oriented patterns
- Pure functions with explicit return types
- Immutable data structures

## 🎯 Universal Design Goals

* Apply **Screaming Architecture**: folder names must reflect the domain intent
* Follow **SOLID** principles and **Clean Code**
* Absolutely **no `any`** allowed
* APIs should be explicit and well-documented
* **Container-First Development**: Use Docker for consistency
* **Prefer functional programming**: favor pure functions, closures, and composition over classes

---

## 📁 Universal Project Structure

```
src/
├── modules/
│   └── {domain}/              # Domain-specific logic
│       ├── routes/            # HTTP route definitions
│       ├── controllers/       # Thin HTTP adapters
│       ├── services/          # Use-cases and business logic
│       ├── schemas/           # DTOs and validations
│       ├── repositories/      # Data access layer
│       ├── mappers/           # Transform external ↔ internal data
│       └── utils/             # Domain-specific utilities
├── shared/
│   ├── middlewares/           # Common middlewares (auth, error, validation)
│   ├── provider/              # External service clients
│   ├── config/                # Environment and configuration
│   └── types/                 # Global types, aliases, enums
└── main.ts                    # Application entry point
```

* Each top-level folder represents a **domain concept** (Screaming Architecture)
* Cross-cutting logic should live in `shared/`
* Use `tsconfig.json` for absolute path aliases

---

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

---

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

// Implementation A
export const databaseUserRepository: UserRepository = {
  findById: (id) => database.findById(id),
  create: (data) => database.create(data),
};

// Implementation B  
export const cacheUserRepository: UserRepository = {
  findById: (id) => cache.findById(id),
  create: (data) => cache.create(data),
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
  // Depends on Repository interface, not specific implementation
  const result = await UserRepository.createUser(userData);
  return result;
};

// ❌ WRONG: Direct dependency on implementation
export const badCreateUserService = async (userData: CreateUserInput) => {
  // Direct dependency on specific client
  const result = await specificClient.users.create(userData);
  return result;
};
```

---

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

---

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

---

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

### **Type Safety Rules**
- **NEVER** use `any` type
- Use `unknown` for dynamic data with type guards
- Define explicit return types for all functions
- Use `Record<string, unknown>` for dynamic objects
- Prefer `type` over `interface` for data shapes

---

## 📦 Technology Standards

### **Dependency Management**
* Use `pnpm` as the exclusive package manager
* For production dependencies: `pnpm add <package>`
* For dev-only dependencies: `pnpm add -D <package>`
* Verify packages are not deprecated and compatible with current stack
* Always refer to official documentation

### **Code Standards**
* Enable `strict: true` and `noImplicitAny: true` in `tsconfig.json`
* Do not use `any`. If dynamic typing is needed, use `unknown` + type guards
* Use `interface` for object shapes, `type` for unions and advanced compositions
* Leverage **generics** for reusable, type-safe APIs
* Prefer **literal unions** over enums
* Never use `as` unless the type safety is proven
* Destructure function arguments to clarify intent
* Favor immutable data and pure functions
* All errors must pass through `errorHandler.middleware.ts`

### **HTTP Client Standards**
* **MANDATORY**: Use centralized HTTP provider - **NEVER** use direct `fetch()` calls
* Use `httpClient` for JSON APIs
* Use specialized clients for specific protocols (OAuth2, GraphQL, etc.)
* Centralize all error handling in middleware

---

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

---

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

---

## 🧪 Testing Patterns

- Services: Unit tests with mocked repositories
- Controllers: Integration tests with request/response
- Repositories: Integration tests with real providers
- Use modern testing frameworks (Vitest, Jest)

---

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

### ✅ Development Quality
- [ ] Controllers have NO try/catch blocks
- [ ] Services use AppError for all exceptions
- [ ] Business logic ONLY in services
- [ ] Data access ONLY in repositories
- [ ] All functions are pure (no side effects except logging)
- [ ] Documentation in English
- [ ] Minimal comments (only JSDoc for functions)

---

## 💡 Prompt Examples (for AI Assistants)

* `Generate a repository for entity management using provider abstraction with strict TypeScript types`
* `Suggest a validation schema for data input with proper error handling`
* `Refactor the service to follow SRP and use dependency injection`
* `Write unit tests for controller using modern testing framework and mock dependencies`
* `Create a mapper that converts entities to DTOs for API responses`

---

> **Mantra**: *Keep controllers thin, services smart, repositories isolated, types explicit, and folder names screaming the business intent. Write code that screams what it does.*

---

## 🐳 Docker Development Environment

* **MANDATORY**: All development must be done through Docker Compose.
* **Stack Location**: Navigate to `../dockers/` directory for full microservices stack.
* **Services**: user-service (3000), mongo-db-365 (27017), r365-mariadb (3306), keycloak (8080).
* **Commands**: 
  * Start: `docker-compose up --build`
  * Logs: `docker-compose logs -f user-service`
  * Rebuild: `docker-compose build --no-cache user-service`
* **Hot Reload**: Changes reflect automatically in containers.
* **Reference**: See `md/docker-development.md` for complete Docker guide.

---

## 📦 Dependency Management

* Use `pnpm` as the exclusive package manager.
* For production dependencies: `pnpm add <package>`.
* For dev-only dependencies: `pnpm add -D <package>`.
* Verify that the package is not deprecated and compatible with July 2025 stack.
* Always refer to official documentation (e.g., Express 5.1, Zod 4.x).
* **Prefer functional programming**: favor pure functions, closures, and composition over classes. Avoid `class` and `constructor` unless absolutely necessary.
* Write code in English, but always speak/discuss in Spanish.

---

## 📝 API Documentation Standards

* **OBLIGATORIO**: Cada módulo/ruta nueva DEBE tener su correspondiente archivo `.yaml` en `src/docs/`
* Usar OpenAPI 3.1 para toda la documentación
* El sistema de documentación dinámico carga automáticamente todos los archivos `.yaml` de `src/docs/`
* Estructura de naming: `src/docs/{module-name}.yaml` (ej: `auth.yaml`, `users.yaml`)

### 🛣️ Proceso para Nuevas Rutas

1. **Crear/Actualizar el endpoint** en el módulo correspondiente
2. **Documentar INMEDIATAMENTE** en el archivo YAML del módulo
3. **Incluir en la documentación**:
   - Descripción clara del endpoint
   - Parámetros de entrada (path, query, body)
   - Todas las respuestas posibles (200, 400, 401, 404, 500, etc.)
   - Ejemplos de request/response
   - Schemas de componentes reutilizables
   - Tags apropiadas para agrupación

### 📋 Template para Nuevos Endpoints

```yaml
/api/v1/{module}/{action}:
  {method}:
    tags: [{ModuleName}]
    summary: "Descripción breve"
    description: "Descripción detallada"
    parameters: # Si aplica
      - in: path/query
        name: paramName
        required: true/false
        schema:
          type: string/number/boolean
        example: "valor-ejemplo"
    requestBody: # Para POST/PATCH/PUT
      required: true
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/SchemaName'
    responses:
      '200':
        description: "Operación exitosa"
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/ResponseSchema'
      '400':
        description: "Datos inválidos"
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/ErrorResponse'
```

---

## 🛠️ Tooling & Automation

* **Container-First Development**: All commands run inside Docker containers.
* **Docker Compose**: Orchestrates user-service, databases (MongoDB/MariaDB), and Keycloak.
* Use `Biome` for formatting and linting: `docker-compose exec user-service pnpm add -D biome`.
* Enforce Conventional Commits using `Commitlint`.
* Enable `strict` mode in `tsconfig.json`.
* Use `Vitest` + `Supertest` for unit and integration testing via `docker-compose exec user-service pnpm test`.
* Configure pre-commit hooks using Husky.
* **Development Workflow**: `cd ../dockers && docker-compose up --build`.

---

## 📚 Code Standards

* Enable `strict: true` and `noImplicitAny: true` in `tsconfig.json`.
* Do not use `any`. If dynamic typing is needed, use `unknown` + type guards.
* Use `interface` for object shapes, `type` for unions and advanced compositions.
* Leverage **generics** for reusable, type-safe APIs.
* Prefer **literal unions** over enums.
* Never use `as` unless the type safety is proven.
* Destructure function arguments to clarify intent.
* Favor immutable data and pure functions.
* All errors must pass through `errorHandler.middleware.ts`.

---

## 📁 Project Structure

```

src/
├── onboarding/                # Domain-specific logic
│   ├── routes/                # Express routes
│   ├── controllers/           # Thin HTTP adapters
│   ├── services/              # Use-cases and business logic
│   ├── schemas/               # DTOs and Zod validations
│   ├── repositories/          # Data access layer
│   ├── mappers/               # Transform external ↔ internal data
│   └── utils/                 # Metamap client, logger, etc.
├── shared/
│   ├── middlewares/           # Common middlewares (auth, error, validation)
│   ├── http/                  # Generic fetch client
│   ├── config/                # env loader, Mongo/MySQL connectors
│   └── types/                 # Global types, aliases, enums
└── index.ts                   # Application entry point

```

* Each top-level folder represents a **domain concept** (Screaming Architecture).
* Cross-cutting logic should live in `shared/`.
* Use `tsconfig.json` for absolute path aliases.

---

## ⚙️ Module Anatomy (`auth/` & `users/`)

### 🛣️ Routes (`routes/auth.routes.ts` & `routes/users.routes.ts`)

* Use `validate(schema)` middleware as the first step in any mutating route.
* Routes should remain logic-free, delegating to controllers.

```ts
// auth routes
.post("/login", validate(loginSchema), loginController)
.post("/register", validate(registerSchema), registerController)
.post("/forgot-password", validate(forgotPasswordSchema), forgotPasswordController)
.patch("/reset-password", validate(resetPasswordSchema), resetPasswordController)
.post("/verify-email", validate(verifyEmailSchema), verifyEmailController)

// users routes
.get("/profile", authenticateToken, getUserProfileController)
.patch("/profile", authenticateToken, validate(updateProfileSchema), updateProfileController)
.patch("/change-password", authenticateToken, validate(changePasswordSchema), changePasswordController)
```

---

### 🎛️ Controller (`controllers/login.controller.ts`)

* Controllers are thin HTTP-to-domain translators.
* Always return `application/json` and use explicit `HttpStatus`.

```ts
export const loginController = async (req: Request, res: Response) => {
  const { email, password } = req.body;
  const authResult = await loginService({ email, password });
  return res.status(HttpStatus.OK).json(authResult);
};
```

---

### 🧠 Service (`services/login.service.ts`)

* Services orchestrate repositories and external APIs.
* Follow SRP: each service exports a single public function.

```ts
export const loginService = async (
  payload: LoginInput
): Promise<AuthResultDTO> => {
  const user = await userRepository.findByEmail(payload.email);
  const isValidPassword = await verifyPassword(payload.password, user.hashedPassword);
  if (!isValidPassword) throw new UnauthorizedError('Invalid credentials');
  const token = await generateJWT({ userId: user._id, email: user.email });
  return { user: userMapper.toDTO(user), token };
};
```

---

### 🗃️ Repository (`repositories/user.mongo.repository.ts`)

* Repositories abstract DB logic and expose domain-friendly methods.

```ts
export const userRepository = {
  findByEmail: async (email: string) => {},
  findById: async (id: string) => {},
  create: async (userData: CreateUserInput) => {},
  updateProfile: async (id: string, updates: UpdateProfileInput) => {},
  updatePassword: async (id: string, hashedPassword: string) => {},
};
```

---

### 🧪 Zod Schema (`schemas/auth.schema.ts`)

* Use `z.object({ body: ... })` for validating Express request bodies.
* Re-export schemas from `schemas/index.ts` for easier import.

```ts
export const loginSchema = z.object({
  body: z.object({
    email: z.string().email(),
    password: z.string().min(8),
  }),
});

export const registerSchema = z.object({
  body: z.object({
    email: z.string().email(),
    password: z.string().min(8),
    firstName: z.string().min(1),
    lastName: z.string().min(1),
  }),
});
```

---

### 🔄 Mapper (`mappers/user.mapper.ts`)

* Transforms database entities into DTOs and vice versa.

```ts
export const userMapper = {
  toDTO: (user: UserEntity): UserDTO => ({
    id: user._id.toString(),
    email: user.email,
    firstName: user.firstName,
    lastName: user.lastName,
    isEmailVerified: user.isEmailVerified,
    createdAt: user.createdAt,
  }),
  
  toEntity: (userInput: CreateUserInput): UserEntity => ({
    email: userInput.email,
    hashedPassword: userInput.hashedPassword,
    firstName: userInput.firstName,
    lastName: userInput.lastName,
    isEmailVerified: false,
    createdAt: new Date(),
  }),
};
```

---

## 🔔 Email Verification & Password Reset

* Email verification and password reset processes use secure tokens with expiration.
* Always respond immediately with success status to prevent email enumeration attacks.
* Use background jobs for sending emails to avoid blocking the response.

```ts
export const forgotPasswordController = async (req: Request, res: Response) => {
  const { email } = req.body;
  await forgotPasswordService({ email });
  // Always return success to prevent email enumeration
  return res.status(HttpStatus.OK).json({ message: 'If email exists, reset link sent' });
};
```

---

## 🧬 Data Strategy

| Phase                | DB      | Reason                                                     |
| -------------------- | ------- | ---------------------------------------------------------- |
| User registration    | MongoDB | Flexible schema, fast writes, suitable for user profiles  |
| Authentication data  | MongoDB | Session management, tokens, and auth-related data         |
| User analytics       | MySQL   | Structured data for reporting and complex queries         |

* Wrap all DB access via repositories for maximum flexibility.

---

## 🚦 Technical Standards

* Use native `fetch()` from Node v24.
* **MANDATORY**: Use centralized HTTP provider from `src/provider/http/` - **NEVER** use direct `fetch()` calls
* Use `httpClient` for JSON APIs, `keycloakHttpClient.postForm()` for OAuth2/form-encoded requests
* Centralize all error handling in middleware.
* Load environment variables from `.env` and expose them via `shared/config/env.ts` with full typing.
* Use dependency injection via constructors or factory functions.
* Generate documentation with `Typedoc`: run `pnpm docs`.
  * Commit messages must follow Conventional Commits:
  `feat(auth): add JWT token validation middleware`.---

## 🚀 Express Bootstrap (`index.ts`)

```ts
const app = express();
app.use(json());
app.use(urlencoded({ extended: true }));
app.use("/api/v1", authRoutes);
app.use("/api/v1", userRoutes);
app.use(errorHandler);
app.listen(process.env.PORT ?? 3000);
```

---

## ⚙️ VSCode & Tooling

* Recommended Extensions: ESLint, Prettier, GitLens, Zod Intellisense.
* Enable IntelliSense for path aliases via `tsconfig.json` + `jsconfig.json`.
* Use Biome for formatting and unified linting.

---

## 💡 Prompt Examples (for Copilot, ChatGPT, Cursor, etc.)

* `Generate a repository for user management using MongoDB with strict TypeScript types.`
* `Suggest a Zod schema for user registration with email validation.`
* `Refactor the auth service to follow SRP and use dependency injection.`
* `Write unit tests for login controller using Vitest and mock dependencies.`
* `Create a mapper that converts user entities to DTOs for API responses.`

---

## 📚 Swagger Documentation System

### 🏗️ Sistema Dinámico de Documentación

* El sistema carga automáticamente todos los archivos `.yaml` de `src/docs/`
* Los combina en una sola especificación OpenAPI 3.1
* Disponible en `http://localhost:3000/docs` durante desarrollo
* Actualización automática al reiniciar el servidor

### 🔧 Configuración Actual

```typescript
// src/docs/index.ts - Sistema de carga dinámica
export const getSwaggerDoc = (): OpenAPIObject => createSwaggerDoc();

// Carga automática de:
// - src/docs/auth.yaml    (Autenticación)
// - src/docs/users.yaml   (Gestión de usuarios)
// - src/docs/*.yaml       (Futuros módulos)
```

### 📋 Reglas de Documentación por Módulo

```yaml
# Estructura obligatoria para cada módulo
src/docs/
├── auth.yaml          # Endpoints de autenticación
├── users.yaml         # Endpoints de usuarios  
├── {nuevo-modulo}.yaml # Cada nuevo módulo DEBE tener su .yaml
└── components/        # Schemas reutilizables (futuro)
```

### 🚀 Workflow para Nuevos Endpoints

1. **Implementar endpoint** en `src/modules/{module}/routes/`
2. **Crear/actualizar** `src/docs/{module}.yaml` INMEDIATAMENTE
3. **Documentar** según template OpenAPI 3.1
4. **Validar** en Swagger UI (`/docs`)
5. **Commit** código y documentación juntos

---

## 🧾 Documentation Guidelines

* `README.md` must include:

  * Quick start guide
  * Architecture diagram
  * Dev/test scripts
  * Swagger usage instructions

* Store architectural decisions in `docs/adr/`.
* Module documentation in `md/{module}.module.md`.

## Functional Programming Only
This project strictly follows functional programming principles. Avoid using object-oriented programming constructs such as class, constructor, this, and inheritance. All logic should be implemented using functions, pure data structures, hooks, and composable patterns. Prefer type and interface over class-based models. 

---

> **Mantra**: *Keep controllers thin, services smart, repositories isolated, types explicit, and folder names screaming the business intent. Document as you code.*
