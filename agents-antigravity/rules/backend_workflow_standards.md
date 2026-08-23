# 🛠️ Rule: Backend Workflow & Clean Standards (Adely's Golden Standards)

> **ESTÁNDAR OBLIGATORIO DE DESARROLLO BACKEND:** Aplica a todo desarrollo backend en Node.js, Bun, Fastify y Express.

---

## 1. 📁 Estructura de Directorios (Public vs Private & Providers)

- **`src/providers/`**: Contiene únicamente proveedores de infraestructura y servicios transversales:
  - `logger.provider.ts` (Pino Logger estructurado).
  - `result.provider.ts` (Result Pattern + helper `sendResult`).
  - `appError.provider.ts` (Safety net para errorHandler global).
  - `database.provider.ts` (ORM / Query Builder).
  - `redis.provider.ts`, etc.
- **`src/modules/public/`**: Contiene los módulos y rutas que NO requieren autenticación previa (ej. `Auth/` con login, register, email validation).
- **`src/modules/private/`**: Contiene los módulos y rutas protegidos que requieren token JWT o sesión activa (ej. `Accounts/`, `Garage/`, `Routes/`, etc.).

---

## 2. ⚡ Result Pattern & Respuestas HTTP Unificadas

1. **Cero Excepciones en Servicios:**
   - Los servicios NUNCA lanzan excepciones (`throw`). Retornan siempre `Promise<Result<T>>`.
   - Utilizan `ResponseOk(data)` para éxito y `ResponseFail.X("ErrorCode")` para fallos (donde `X` representa el tipo de error HTTP: `BadRequest`, `Unauthorized`, `NotFound`, `UnprocessableEntity`, `Conflict`, `Internal`, etc.).
2. **Helper `sendResult` en Handlers/Controllers:**
   - Toda respuesta HTTP enviada por el servidor sigue estrictamente el mismo esquema JSON:
     - **Éxito (200 / 201):** `{ "message": "...", "data": T }`
     - **Fallo (4xx / 5xx):** `{ "error": "ErrorCode" }`

---

## 3. 🪵 Logs Estructurados (`src/providers/logger.provider.ts`)

- Usar exclusivamente Pino Logger para emitir logs de servidor (`logger.info`, `logger.warn`, `logger.error`).
- Loguear **únicamente lo estrictamente necesario** (inicio de servicios, fallos críticos con contexto, operaciones clave).
- Salida JSON estructurada en producción y formateada (pretty) en desarrollo.
- Queda estrictamente prohibido loguear contraseñas, tokens JWT completos o datos personales sensibles.

---

## 🧪 4. Colección de Pruebas Bruno (`.bru`) Obligatoria

- Por cada endpoint HTTP creado o modificado, se DEBE crear un archivo `.bru` en la carpeta `bruno/` correspondiente (`bruno/Public/` o `bruno/Private/`).
- El archivo `.bru` debe respetar la estructura completa de Bruno:
  - `meta`: nombre, secuencia y tipo (`http`).
  - `method` (get/post/put/delete): URL con variable `{{base_url}}`.
  - `headers`: Content-Type application/json.
  - `body:json`: payload de prueba.
  - `docs`: explicación del propósito del endpoint.
  - `example`: ejemplo de request y respuesta exitosa.
