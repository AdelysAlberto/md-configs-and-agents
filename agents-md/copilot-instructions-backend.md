# 🧠 Copilot Instructions – Backend Screaming Architecture

## 📚 Documentation Index

This document provides the foundational guidelines for the User Management service. For detailed implementation guidance, refer to these specialized documents:

- **[Development Patterns & Guidelines](./copilot-development-patterns.md)** - Mandatory patterns and coding standards
- **[Implementation Guide](./copilot-implementation-guide.md)** - Step-by-step feature development
- **[SOLID & Clean Architecture](./solid-clean-architecture.md)** - Architecture principles and best practices

---

## 📁 Current Project Structure

src/
├── auth/                      # Authentication & authorization logic
│   ├── routes/                # Express routes for auth
│   ├── controllers/           # Thin HTTP adapters
│   ├── services/              # Use-cases and business logic
│   ├── schemas/               # DTOs and Zod validations
│   ├── repositories/          # Data access layer
│   ├── mappers/               # Transform external ↔ internal data
│   └── utils/                 # JWT client, encryption, etc.
├── users/                     # User management logic
│   ├── routes/                # Express routes for users
│   ├── controllers/           # Thin HTTP adapters
│   ├── services/              # Use-cases and business logic
│   ├── schemas/               # DTOs and Zod validations
│   ├── repositories/          # Data access layer
│   ├── mappers/               # Transform external ↔ internal data
│   └── utils/                 # User utilities, validators, etc.
├── shared/
│   ├── middlewares/           # Common middlewares (auth, error, validation)
│   ├── http/                  # Generic fetch client
│   ├── config/                # env loader, Mongo/MySQL connectors
│   └── types/                 # Global types, aliases, enums
└── index.ts                   # Application entry point

````## 🧩 Project Scope

* This document applies **only to the backend** of the **User Management** service.
* Technologies: `Node v24.3`, `Express 5.1`, `TypeScript 5.x`, `Zod 4.x`, `native fetch`, `MongoDB 5.7`.

## 🎯 Design Goals

* Apply **Screaming Architecture**: folder names must reflect the domain intent.
* Follow **SOLID** principles and **Clean Code**.
* Absolutely **no `any`** allowed.
* API should be WCAG 2.2-friendly and explicit.

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

* Use `Biome` for formatting and linting: `pnpm add -D eslint biome`.
* Enforce Conventional Commits using `Commitlint`.
* Enable `strict` mode in `tsconfig.json`.
* Use `Vitest` + `Supertest` for unit and integration testing.
* Configure pre-commit hooks using Husky.

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

````

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
````

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
