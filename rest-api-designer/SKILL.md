---
name: rest-api-designer
description: Design a clean, consistent REST API — resources, methods, status codes, errors, pagination, versioning. Use when designing a new endpoint or API surface, reviewing an API design, or asked "design this API", "what should this endpoint look like", "review my REST API". Consistency over cleverness.
---

# REST API Designer

An API is a contract you'll live with for years. Design it consistent and predictable so
clients can guess the next endpoint correctly. Boring and uniform beats clever and unique.

## Resources & methods

- **Nouns, not verbs**, plural: `/orders`, `/orders/{id}`, `/orders/{id}/items`. The action
  is the HTTP method, not the path (`POST /orders`, not `/createOrder`).
- **Method semantics**: GET (read, safe, idempotent), POST (create/non-idempotent action),
  PUT (full replace, idempotent), PATCH (partial update), DELETE (idempotent).
- Nesting one level deep max; beyond that, use query params or top-level resources with filters.

## Status codes (use the specific one)

- 200 OK · 201 Created (+ `Location`) · 204 No Content (delete/empty) · 202 Accepted (async).
- 400 bad input · 401 unauthenticated · 403 authenticated-but-forbidden · 404 not found ·
  409 conflict · 422 validation · 429 rate-limited (+ `Retry-After`).
- 5xx = server's fault, never for user error. Don't return 200 with an error body.

## Errors (consistent shape, always)

```
{ "error": { "code": "order_not_found", "message": "human-readable", "details": [...] } }
```
A stable machine `code` clients can branch on; a message for humans; field-level `details`
for validation. Same shape for every error in the whole API.

## Cross-cutting (decide once, apply everywhere)

- **Pagination**: cursor-based for large/changing sets (`?limit=&cursor=`), return the next
  cursor. Offset only for small stable sets. Never return unbounded lists.
- **Filtering/sorting**: query params, documented, allowlisted (`?status=paid&sort=-created`).
- **Versioning**: pick one (URL `/v1` or header) and hold it. Version on breaking changes only.
- **Idempotency** for POST that creates money/side-effects: accept an `Idempotency-Key`.
- **Auth** stated per endpoint; **rate limits** communicated via headers.

## Rules

- Consistency beats local cleverness — a predictable API is a usable API.
- Never break a published contract without a version bump. Additive changes are safe; removals/renames aren't.
- Design the error and pagination shapes ONCE, up front — retrofitting them is painful.

## Output

The resource model, endpoints (method + path + purpose), the standard error shape, pagination
+ versioning decision, and auth per endpoint. Flag any breaking change if reviewing an existing API.
