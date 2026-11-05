# AgentClinic Roadmap

## Principles

- Work in tiny vertical slices; each phase delivers one visible, end-to-end capability.
- Keep `main` runnable at the end of every phase.
- Build the real flow, not placeholders that are thrown away later.
- Add tests with the feature, not in a separate cleanup phase.
- Resist scope creep: finish a phase before starting the next.

## Phase 0 — Specs and repository skeleton

Create the constitution, establish `server/` and `web/` directories, and remove the placeholder `src/index.ts`.

**Done when:** the repository builds from a clean checkout and the README points to `specs/`.

## Phase 1 — Walking skeleton

Add a Go/Gin API with a health endpoint, a Vite React shell, a dev proxy, and a production build where Gin serves the client.

**Done when:** the browser loads the app shell and displays live API health.

## Phase 2 — Agents list

Seed agents in memory, expose `GET /api/v1/agents`, and render a dashboard table with name, model, human, and status.

**Done when:** the list shows real API data with loading, empty, and error states.

## Phase 3 — Agent detail

Add `GET /api/v1/agents/:id` and an agent detail page with profile and chief complaint.

**Done when:** deep links work and an unknown agent ID shows a friendly not-found page.

## Phase 4 — Ailments catalog

Add ailment list and detail endpoints and pages, and link agents to their ailments.

**Done when:** a visitor can browse ailments and see which agents are affected.

## Phase 5 — Therapies catalog

Add therapy list and detail endpoints and pages, and map therapies to the ailments they treat.

**Done when:** an ailment page shows its recommended therapies.

## Phase 6 — Appointment booking

Add validated `POST /api/v1/appointments`, a booking form, and a confirmation screen.

**Done when:** an agent can book an appointment, and invalid input is rejected with accessible errors.

## Phase 7 — Staff schedule

Add a day view of appointments plus status transitions: booked, checked-in, completed, and canceled.

**Done when:** staff can view the day's schedule and update an appointment's status.

## Phase 8 — Persistence

Add a repository-backed store, migrations, and a seed command, then replace the in-memory adapter without changing handlers or client contracts.

**Done when:** data survives a restart and all existing tests still pass.

## Phase 9 — Design and accessibility polish

Add a marketing landing page, responsive layouts, and consistent empty, loading, and error states.

**Done when:** the app passes a WCAG 2.1 AA checklist and works on mobile widths.

## Phase 10 — Hardening and demo readiness

Complete unit, API, and end-to-end coverage; add CI; document setup and reset of sample data.

**Done when:** one command starts a working demo, CI is green, and a short demo script exists.
