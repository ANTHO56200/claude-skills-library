---
name: mcp-builder
description: Scaffold a correct, secure MCP (Model Context Protocol) server with the patterns that generic generators miss — auth, pagination, errors, and a test. Use when building an MCP server/tool, exposing an API to an agent, or "make an MCP server for X", "add an MCP tool".
---

# MCP Builder

Build an MCP server an agent can actually use safely: typed tools, real error handling,
credential hygiene, and a test that proves it works. Skip the toy scaffold.

## Tool design (per tool)

- **Name + description**: the description is the agent's only guide — say what it does AND
  when to use it, concretely. Vague descriptions = wrong tool calls.
- **Input schema**: strict JSON Schema, required vs optional explicit, enums over free
  strings where possible, sane bounds (limits, max lengths). Validate on entry.
- **Output**: structured and predictable. Return typed data, not a giant string the model
  must parse. Keep payloads small — trim fields the agent doesn't need.

## Patterns generic generators miss

- **Auth / credentials**: read from env, never hardcode; never echo the key back in output
  or errors. Fail clearly if unconfigured.
- **Pagination**: expose `limit`/`cursor`, don't dump 10k rows into context. Default small.
- **Errors**: catch and return a clear, actionable message ("rate limited, retry after N"),
  not a raw stack trace. Distinguish user-fixable vs server errors.
- **Rate limits / retries**: backoff on the upstream API; surface the limit to the agent.
- **Idempotency**: for write tools, make retries safe or say they aren't.

## Security checklist

- Least privilege: only the tools the job needs.
- No arbitrary shell/eval exposed as a tool. No user-string → command.
- Validate and bound every input before it hits the upstream/DB.
- Secrets only via env; `.env` git-ignored; `.env.example` documents the names.

## Ship with a test

A minimal test that starts the server, calls each tool with a valid + an invalid input,
and asserts the shapes. A tool with no test is a tool you haven't run.

## Output

The server scaffold (tools + schemas + handlers), env setup, and the test file.
State the language/SDK and any upstream API assumptions.
