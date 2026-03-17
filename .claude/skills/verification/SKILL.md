---
name: verification
description: Run backpressure checks (lint, typecheck, tests, build) for backend and/or frontend. Use after any code change to verify nothing is broken.
argument-hint: <backend|frontend|all>
allowed-tools: Bash
---

Run quality checks for the specified component. ALL checks must pass before any commit.

## Backend (cd backend)

```bash
uv run ruff check .              # Lint (auto-fix with: uv run ruff check --fix .)
uv run mypy .                    # Type check — strict mode
uv run pytest                    # Tests (requires Docker for MQTT testcontainers)
```

Run in this order. If any check fails, fix the issue and re-run ALL checks from the beginning.

**Note:** `uv run pytest` requires Docker to be running (Mosquitto MQTT broker via testcontainers).

## Frontend (cd frontend)

```bash
npm run lint                     # ESLint
npm run build                    # TypeScript check + Vite production build
```

## Browser E2E (frontend/fullstack tasks only)

After all checks pass, verify in a real browser using Chrome DevTools MCP:
1. Start backend: `cd backend && docker-compose up -d`
2. Start frontend: `cd frontend && npm run dev`
3. Navigate, interact, check for console errors
4. For visual tasks: compare against Figma design (URL in verification-config.json)

## Figma Visual Verification (frontend tasks only)

Use Figma MCP tools (`get_design_context`, `get_screenshot`) to compare implementation against design.
URL: https://www.figma.com/design/wYwJVRnlPhF9uplIawpl0i/ISIC-Attendance--Copy-?node-id=616-41377&t=mYni0sAk4toIPrXX-1
