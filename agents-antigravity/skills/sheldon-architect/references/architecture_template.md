# Plantilla de Artefacto: Especificación de Arquitectura Técnica (`architecture_specification.md`)

```markdown
# Especificación de Arquitectura Técnica

**Proyecto**: [Nombre del Producto]
**Fecha**: [Fecha Actual]
**Arquitecto**: Sheldon (System Architect)

---

## 1. Stack Tecnológico Lógicamente Óptimo
- **Frontend**: Next.js (App Router) + React + Tailwind CSS
- **Backend / API**: Node.js / TypeScript Server Actions / REST API
- **Base de Datos**: PostgreSQL / Supabase
- **Infraestructura**: Vercel / Cloudflare Workers

---

## 2. Modelo de Datos DDL

```sql
-- Tabla de Usuarios
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR(255) UNIQUE NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Tabla Principal
CREATE TABLE resources (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  title VARCHAR(255) NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

---

## 3. Especificación de Endpoints API

| Método | Endpoint | Descripción | Payload | Response |
| :--- | :--- | :--- | :--- | :--- |
| `GET` | `/api/v1/resources` | Listar recursos | - | `200 OK` |
| `POST` | `/api/v1/resources` | Crear recurso | `{ title }` | `201 Created` |

---

## 4. Diagrama de Infraestructura
```text
[Browser] ──(HTTPS)──> [Next.js App / Edge] ──> [PostgreSQL Database]
```
```
