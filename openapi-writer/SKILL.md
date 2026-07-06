---
name: openapi-writer
description: Write or validate an OpenAPI (Swagger) spec from an API or a design. Use when documenting an API, generating a client/server from a spec, or asked "write the OpenAPI spec", "document these endpoints", "validate my swagger". Produces a spec that's actually complete enough to generate from.
---

# OpenAPI Writer

An OpenAPI spec is only useful if it's complete and accurate — a half-spec generates broken
clients and lies to consumers. Ground it in the real endpoints; describe every response.

## Build it from the real API

Read the actual routes, handlers, and models — don't invent fields. For each endpoint capture:
- Path + method, summary + description (what AND when to use it).
- **Parameters**: path/query/header, each with type, required, description, example.
- **Request body**: schema (`$ref` a reusable component), required fields, example.
- **Responses**: EVERY status it returns — 200 AND the 4xx/5xx with their error schema.
  A spec that only documents the happy path is half a spec.

## Structure well

- **Components/schemas**: define models once, `$ref` them everywhere. No copy-pasted inline schemas.
- **Reusable responses/parameters**: the standard error, the pagination params — define once.
- **Security schemes**: declare auth (bearer/apiKey/oauth) and apply per operation.
- **Examples**: realistic ones on requests and responses — they drive the docs UI and mocks.

## Validate

- Spec parses and is valid OpenAPI 3.x (no dangling `$ref`, no missing required fields).
- Every path/method the code exposes is present; nothing documented that doesn't exist.
- Types match reality (the field the API returns as a string isn't specced as integer).

## Rules

- Document every response status, not just 200 — consumers need the error shapes to handle them.
- `$ref` shared schemas; duplication drifts out of sync.
- If you can't confirm a field from the code, mark it and ask — don't guess types into a contract.

## Output

The OpenAPI 3.x document (YAML or JSON as the project uses), with components, security, and
examples. Note any endpoint you couldn't fully verify against the code.
