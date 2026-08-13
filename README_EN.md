# HotKey Web (app)

[![CI](https://github.com/StephenQiu30/hotkey-web/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/StephenQiu30/hotkey-web/actions/workflows/ci.yml)

The **app (Web client workspace)** of the HotKey platform, providing content creators with content archiving, workspace visualization, real-time hotspot monitoring, and more. This repository contains only the frontend application; the backend lives in the separate [`hotkey-server`](https://github.com/StephenQiu30/hotkey-server) repository, and this app consumes its published OpenAPI contract.

## Tech stack

Next.js App Router · React · TypeScript · Tailwind CSS · Radix UI · zustand · axios · recharts · GSAP

## Directory structure

```
hotkey-web/
├── src/
│   ├── app/            # Next.js App Router (page files)
│   ├── components/     # Business and UI composite components
│   ├── layouts/        # Workspace layouts
│   ├── stores/         # zustand state management
│   ├── lib/            # Requests, auth session, and utilities
│   └── services/       # Auto-generated OpenAPI client
├── public/             # Brand and static assets
├── test/               # Unit tests and shared test setup
└── ...
```

## Development

Prerequisite: Node.js (prefer the version pinned in this repo). You need a reachable `hotkey-server` backend instance (see its README to start one).

```bash
git clone https://github.com/StephenQiu30/hotkey-web.git
cd hotkey-web
npm ci
cp .env.example .env
npm run dev
```

It starts at `http://127.0.0.1:3000` by default and reaches the backend through Next.js server-side rewrites. Environment variables are documented in [.env.example](.env.example).

## OpenAPI collaboration

- The published contract comes from `hotkey-server`'s `docs/openapi/swagger.json`.
- When the backend contract changes: regenerate OpenAPI on the backend, then run `npm run openapi:generate` to regenerate the client.
- Run `npm run openapi:check` before committing to confirm no drift between the contract and the client.
- Business code only calls generated functions under `src/services/hotkey/hotkey-server/`; never hand-write backend DTOs or endpoint paths.

## Quality gates

```bash
npm run openapi:check
npm run typecheck
npm run test:unit
npm audit --omit=dev --audit-level=high
npm run build
```

See [CONTRIBUTING.md](CONTRIBUTING.md) and [SECURITY.md](SECURITY.md) for contribution and vulnerability-reporting guidance.
