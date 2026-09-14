# Indus University Smart Classroom Finder

Students can find rooms free from verified timetable occupancy, browse schedules, and submit timetables for review.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/indus-classroom-finder/src/App.tsx` — responsive finder, repository, submission, and protected admin surfaces.
- `artifacts/api-server/src/routes/classroom.ts` — availability, free-window, repository, submission, and admin API routes.
- `artifacts/api-server/src/lib/classroom-seed.ts` — initial CSE Semester 3 verified dataset and observed room list.
- `lib/db/src/schema/classroom.ts` — PostgreSQL schema for departments, rooms, timetables, entries, and submissions.
- `lib/api-spec/openapi.yaml` — source of truth for generated API hooks and validation schemas.
- `artifacts/indus-classroom-finder/src/index.css` — shared visual theme and Clerk styling layer.

## Architecture decisions

- Room availability is computed as active master rooms minus occupied rooms from verified timetables, never from rooms that happen to appear in one schedule.
- Availability responses expose `roomDatabaseComplete` and a plain-language disclaimer because the initial room list is known to be incomplete.
- The initial dataset is seeded once for CSE Semester 3 / 2026-27; new departments and submissions use the same normalized timetable tables.
- Clerk protects the admin summary and review queue while the finder, repository, and submission entry point remain public.

## Product

- Multi-section room finder with day, period, academic year, and verified-data filtering.
- Dynamic best free windows, timetable repository with weekly detail, and contribution form with manual/file-source options.
- Protected admin overview showing verification queue and coverage status.

## User preferences

- Keep the experience mobile-friendly, fast to scan, and explicit about what “free” means.

## Gotchas

- Clerk development-key warnings in the browser are expected in development; do not treat them as production failures.
- The room database is intentionally marked incomplete until the university master room list is added.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
