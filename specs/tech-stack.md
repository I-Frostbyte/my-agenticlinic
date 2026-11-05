# AgentClinic Tech Stack

## Decision summary

| Concern | Choice |
| --- | --- |
| Client | TypeScript + React (Vite) |
| Server | Go + Gin |
| API style | REST/JSON under `/api/v1` |
| Styling | Tailwind CSS |
| Storage | Repository abstraction; engine chosen before the persistence phase |
| Deployment | One Go binary serving the API and the built client |

TypeScript remains the language of the dashboard and of all client-side contracts, honoring Mary's request for a TypeScript-based, popular stack. The server is Go with Gin per the project owner's direction. This keeps the client ecosystem familiar while giving the API a small, fast, reliable runtime.

## Client

- TypeScript 5.x in `strict` mode; no implicit `any`.
- React 18 with function components and hooks.
- Vite for development and production builds.
- React Router for page routing and deep links.
- Tailwind CSS for styling, with a small shared component layer.
- Typed `fetch` wrapper for API calls; no global state library until a real need appears.
- Accessible, responsive layouts targeting WCAG 2.1 AA.

## Server

- Go 1.22 or newer.
- Gin for routing, middleware, and request binding.
- Standard library `log/slog` for structured logging.
- `go-playground/validator` through Gin binding for request validation.
- Configuration from environment variables, with a checked-in `.env.example`.
- JSON errors in a consistent envelope:

```json
{
  "error": {
    "code": "agent_not_found",
    "message": "Agent not found.",
    "details": {}
  }
}
```

- In production, Gin serves the built client assets so the app ships as one binary.

## Architecture

A modular monolith with clear seams:

```text
server/
  cmd/api/          entrypoint and wiring
  internal/domain/  entities and repository interfaces
  internal/http/    handlers, middleware, request/response types
  internal/store/   repository implementations
web/
  src/features/     feature-based UI modules
  src/components/   shared presentational components
  src/lib/          API client and utilities
specs/              constitution and specs
```

Rules:

- Handlers depend on domain interfaces, never on a concrete database.
- SQL, if any, stays inside `internal/store`.
- Shared API types are defined once on the client and mirrored explicitly by Go structs.
- IDs are opaque strings; timestamps are RFC 3339 UTC.
- JSON fields use `camelCase`.

## Data

- Persistence is hidden behind repository interfaces from the first phase.
- Early phases use an in-memory adapter seeded with sample agents, ailments, therapies, and appointments.
- The concrete database engine is selected before the persistence phase; swapping it must not change handlers or client contracts.

## Testing and quality

- Server: `testing` plus `httptest`, with table-driven unit and handler tests.
- Client: Vitest and React Testing Library for components and hooks.
- End-to-end: Playwright once the booking flow exists.
- Formatting and linting: `gofmt`, `go vet`, ESLint, and Prettier.
- CI runs build, lint, and tests on every change.

## Developer workflow

- `make dev` runs the Go API and Vite dev server together.
- `make test` runs server and client tests.
- `make build` compiles the client and the Go binary.
- `make seed` loads sample data once persistence exists.
- Dependencies are pinned with `go.mod` and `package-lock.json`.

## Constraints

- Support current evergreen browsers.
- No secrets in the repository; only `.env.example` is committed.
- Prefer standard library and small dependencies over large frameworks.
- Keep the repo runnable with one documented setup command.
