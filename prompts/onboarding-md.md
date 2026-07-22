# 🧠 Copilot Instructions – Backend Screaming Architecture 

## 🧩 Project Scope

* This document applies **only to the backend** of the **Onboarding KYC** service.
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

## ⚙️ Module Anatomy (`onboarding/`)

### 🛣️ Routes (`routes/onboarding.routes.ts`)

* Use `validate(schema)` middleware as the first step in any mutating route.
* Routes should remain logic-free, delegating to controllers.

```ts
.post("/onboarding", validate(startOnboardingSchema), startOnboarding)
.post("/webhooks/metamap", validate(metamapWebhookSchema), metamapWebhook)
````

---

### 🎛️ Controller (`controllers/startOnboarding.controller.ts`)

* Controllers are thin HTTP-to-domain translators.
* Always return `application/json` and use explicit `HttpStatus`.

```ts
export const startOnboarding = async (req: Request, res: Response) => {
  const { userId, email } = req.body;
  const onboarding = await startOnboardingService({ userId, email });
  return res.status(HttpStatus.CREATED).json(onboarding);
};
```

---

### 🧠 Service (`services/startOnboarding.service.ts`)

* Services orchestrate repositories and external APIs.
* Follow SRP: each service exports a single public function.

```ts
export const startOnboardingService = async (
  payload: StartOnboardingInput
): Promise<OnboardingDTO> => {
  const onboarding = await onboardingRepository.createInitial(payload);
  const session = await metamapClient.createSession({ ... });
  return onboardingRepository.updateSession(onboarding._id, session.id);
};
```

---

### 🗃️ Repository (`repositories/onboarding.mongo.repository.ts`)

* Repositories abstract DB logic and expose domain-friendly methods.

```ts
export const onboardingRepository = {
  createInitial: async () => {},
  updateSession: async () => {},
  saveFinal: async () => {},
};
```

---

### 🧪 Zod Schema (`schemas/onboarding.schema.ts`)

* Use `z.object({ body: ... })` for validating Express request bodies.
* Re-export schemas from `schemas/index.ts` for easier import.

```ts
export const startOnboardingSchema = z.object({
  body: z.object({
    userId: z.string().uuid(),
    email: z.string().email(),
  }),
});
```

---

### 🔄 Mapper (`mappers/metamap-to-domain.mapper.ts`)

* Transforms third-party payloads into your internal model.

```ts
export const mapMetamapWebhook = (payload: MetamapWebhook): KycResult => ({
  kycId: payload.id,
  status: payload.status,
  documentType: payload.documents?.[0]?.type ?? "unknown",
});
```

---

## 🔔 Webhooks

* `POST /webhooks/metamap` is used by Metamap to deliver KYC results.
* Always respond immediately with `200 OK` to prevent retries.
* Dispatch async processing (e.g., via message queue or background worker).

```ts
export const metamapWebhook = async (req: Request, res: Response) => {
  queue.dispatch("PROCESS_METAMAP_WEBHOOK", req.body);
  return res.sendStatus(HttpStatus.OK);
};
```

---

## 🧬 Data Strategy

| Phase              | DB      | Reason                                                   |
| ------------------ | ------- | -------------------------------------------------------- |
| Onboarding started | MongoDB | Flexible schema, fast writes, suitable for temp storage  |
| KYC completed      | MySQL   | Structured relational data, useful for analytics & joins |

* Wrap all DB access via repositories for maximum flexibility.

---

## 🚦 Technical Standards

* Use native `fetch()` from Node v24.
* Centralize all error handling in middleware.
* Load environment variables from `.env` and expose them via `shared/config/env.ts` with full typing.
* Use dependency injection via constructors or factory functions.
* Generate documentation with `Typedoc`: run `pnpm docs`.
* Commit messages must follow Conventional Commits:
  `feat(onboarding): add Metamap webhook handler`.

---

## 🚀 Express Bootstrap (`index.ts`)

```ts
const app = express();
app.use(json());
app.use(urlencoded({ extended: true }));
app.use("/api/v1", onboardingRoutes);
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

* `Generate a repository for onboarding using MongoDB with strict TypeScript types.`
* `Suggest a Zod schema for Metamap webhook payload version 3.`
* `Refactor the onboarding service to follow SRP and use dependency injection.`
* `Write unit tests for onboarding controller using Vitest and mock dependencies.`
* `Create a mapper that converts Metamap payloads to the internal KYCResult model.`

---

## 🧾 Documentation Guidelines

* `README.md` must include:

  * Quick start guide
  * Architecture diagram
  * Dev/test scripts

* Store architectural decisions in `docs/adr/`.

---

> **Mantra**: *Keep controllers thin, services smart, repositories isolated, types explicit, and folder names screaming the business intent.*