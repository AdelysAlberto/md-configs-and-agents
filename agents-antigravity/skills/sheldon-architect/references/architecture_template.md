# Artifact Template: Technical Architecture Specification (`architecture_specification.md`)

```markdown
# Technical Architecture Specification

**Project**: [Product Name]
**Date**: [Current Date]
**Architect**: Sheldon (System Architect)

---

## 1. Logically Optimal Technology Stack
- **Frontend**: Next.js (App Router) + React + Tailwind CSS
- **Backend / API**: Node.js / TypeScript Server Actions / REST API
- **Database**: PostgreSQL / Supabase
- **Infrastructure**: Vercel / Cloudflare Workers

---

## 2. DDL Data Model

```sql
-- Users Table
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR(255) UNIQUE NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Main Table
CREATE TABLE resources (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  title VARCHAR(255) NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

---

## 3. API Endpoint Specification

| Method | Endpoint | Description | Payload | Response |
| :--- | :--- | :--- | :--- | :--- |
| `GET` | `/api/v1/resources` | List resources | - | `200 OK` |
| `POST` | `/api/v1/resources` | Create resource | `{ title }` | `201 Created` |

---

## 4. Infrastructure Diagram
```text
[Browser] ──(HTTPS)──> [Next.js App / Edge] ──> [PostgreSQL Database]
```
```
