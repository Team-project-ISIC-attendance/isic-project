# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ISIC Project is a student identification and attendance tracking system with three components:
- **Frontend** (`isic-project-frontend/`): React 19 + TypeScript + Vite + React Router
- **Backend** (`isic-project-backend/`): FastAPI + async SQLAlchemy + MQTT + SQLite
- **Hardware** (`isic-project-hardware/`): ESP8266/ESP32 firmware (C++17, PlatformIO) with PN532 NFC reader

The flow: Hardware scans ISIC cards via NFC → publishes JSON to MQTT topic `isic/scan` → Backend receives, auto-creates ISIC records, stores scans → Frontend displays data via REST API.

## Commands

### Backend (`cd isic-project-backend`)

```bash
uv sync                              # Install dependencies
alembic upgrade head                 # Run database migrations
uv run python -m src.main            # Start the app (requires HTTP_HOST and HTTP_PORT env vars)
docker-compose up                    # Start backend + MQTT broker (ports 8000, 1883)

uv run ruff check --fix .            # Lint and autofix
uv run mypy .                        # Type check (strict mode)
uv run pytest                        # Run tests (requires Docker for MQTT container)
alembic revision --autogenerate -m "description"  # Create migration
```

### Frontend (`cd isic-project-frontend`)

```bash
npm install                          # Install dependencies
npm run dev                          # Start Vite dev server
npm run build                        # TypeScript check + Vite production build
npm run lint                         # ESLint
```

### Hardware (`cd isic-project-hardware`)

```bash
pio run                              # Build for ESP8266 (production)
pio run -e esp8266_debug             # Build debug variant
pio run -t upload                    # Flash to device
pio device monitor                   # Serial monitor
```

## Architecture

### Backend

Layered async architecture: `api/routes.py` → `services/scan_service.py` → `models/` → `database/connection.py`

- **Lifespan**: FastAPI app starts MQTT client as a background task on startup (`src/main.py`)
- **MQTT flow**: `mqtt/client.py` subscribes and loops, dispatches to `mqtt/handler.py`, which calls `scan_service` to create records
- **Database**: Async SQLAlchemy with aiosqlite. Two models: `ISIC` (cards) and `ISICScan` (scan events, FK to ISIC). Alembic manages migrations.
- **Config**: Pydantic Settings reads env vars (`src/config.py`)

### Hardware

Event-driven architecture built on a custom Signal/Slot EventBus:

- **EventBus** (`core/EventBus.hpp`): Central pub/sub with per-event-type `Signal<T>` dispatch. Services communicate exclusively through events — no direct service-to-service calls.
- **Services**: All implement `IService` with lifecycle (`begin()` → `loop()` → `end()`). Key services: `Pn532Service` (NFC), `MqttService` (publish with offline buffer), `AttendanceService` (debounce/batch), `PowerService` (smart sleep modes).
- **TaskScheduler**: Cooperative multitasking — all services are non-blocking.
- **App** (`App.cpp`): Main coordinator that owns all services and the EventBus.

## Code Style

### Backend (Python)
- **No** `from __future__ import annotations`
- **No** `TYPE_CHECKING` patterns — use direct imports
- Imports only at top of file, never inside functions
- Forward references: string literals with `# type: ignore[name-defined]` and `# noqa: F821`
- ruff: line-length 88, target Python 3.13
- mypy: strict mode with SQLAlchemy plugin

### Frontend (TypeScript)
- Strict TypeScript (`noUnusedLocals`, `noUnusedParameters`)
- ESLint 9 flat config with React Hooks and React Refresh plugins

### Hardware (C++)
- C++17 standard (gnu++2a build flag)
- Header-only service declarations in `include/`, implementations in `src/`

## Git Workflow

Git Flow model with `master` (production) and `develop` (integration) branches. All work in feature/fix branches, merged via PRs with `--no-ff`. Conventional commits: `feat:`, `fix:`, `refactor:`, `test:`, `docs:`, `chore:`. No direct commits to `develop` or `master`. No force push. No AI attribution in commits.