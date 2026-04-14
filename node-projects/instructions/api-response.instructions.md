---
applyTo: "**/controllers/**, **/interfaces/**, **/routes/**, **/schemas/**"
---

# API Response Format — Mandatory Standard

## Response Structure Rules

| Rule | Details |
|------|---------|
| `data` | Contains content directly — array `[...]` for lists, object `{...}` for single items |
| `pagination` | ALWAYS a **sibling** of `data`, at the same level |
| `status` | NEVER in HTTP body — only in HTTP header |
| `error` | Single field `{ "error": "ErrorCode" }` for errors |

## ✅ Correct Responses

```json
// Single item
{
  "message": "Contact details retrieved successfully",
  "data": {
    "id": "53e0bcd8-...",
    "status": "ACCEPTED",
    "isFavorite": false
  }
}

// List with pagination
{
  "message": "Contacts retrieved successfully",
  "data": [
    { "id": "53e0bcd8-...", "status": "ACCEPTED" }
  ],
  "pagination": {
    "total": 2,
    "limit": 20,
    "offset": 0,
    "hasMore": false
  }
}

// Error
{
  "error": "UserNotFound"
}
```

## ❌ Forbidden Patterns

```json
// ❌ data wrapped inside extra key
{ "data": { "contacts": [...] }, "pagination": {...} }
{ "data": { "items": [...] }, "pagination": {...} }

// ❌ status inside body
{ "data": {...}, "status": 200 }

// ❌ pagination nested inside data
{ "data": { "items": [...], "pagination": {...} } }
```

## Implementation

**Services** return `Result<T>`:
- Simple: `ResponseOk(data)`
- Paginated: `ResponseOk({ items, total })` — controller extracts the array

**Controllers** — two patterns:
- **Simple**: `sendResult(res, result, "message")`
- **Paginated**: destructure `result.data`, use `createSuccessResponse()` + `createPaginationMeta()`

→ See `controllers.instructions.md` for full examples.
