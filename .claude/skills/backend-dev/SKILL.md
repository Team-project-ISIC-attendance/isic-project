---
name: backend-dev
description: Start and develop the ISIC Project backend (FastAPI + async SQLAlchemy + MQTT + SQLite). Use when starting the backend server, running docker-compose, writing API routes, working with database models/migrations, debugging MQTT message handling, running tests, or doing any backend development work.
---

# Backend Development

## Quick Reference

**Start everything (backend + MQTT broker):**
```bash
cd backend && docker-compose up --build
```
Backend at `http://localhost:8000`, MQTT broker at `localhost:1883`.

**Start locally (without Docker):**
```bash
cd backend && HTTP_HOST=0.0.0.0 HTTP_PORT=8000 uv run python -m src.main
```
Requires a running MQTT broker separately.

**Other commands:**
```bash
uv sync                              # Install/sync dependencies
alembic upgrade head                 # Run database migrations
uv run ruff check --fix .            # Lint and autofix
uv run mypy .                        # Type check (strict mode)
uv run pytest                        # Run tests (requires Docker for MQTT container)
alembic revision --autogenerate -m "description"  # Create new migration
```

## Startup Workflow

### Option A: Docker Compose (recommended)

This starts both the backend and MQTT broker together:

```bash
cd backend && docker-compose up --build
```

The `docker-entrypoint.sh` automatically runs `alembic upgrade head` before starting the app.

**Services:**

| Service | Port | Details |
|---------|------|---------|
| Backend (FastAPI) | `8000` | Health check: `GET /health` every 30s |
| MQTT (Mosquitto 2.0) | `1883` | Anonymous access, no persistence |

**Volumes:**
- `./data:/app/data` — persistent SQLite database

### Option B: Local development (no Docker)

1. Install dependencies:
   ```bash
   cd backend && uv sync
   ```

2. Run migrations:
   ```bash
   cd backend && alembic upgrade head
   ```

3. Start a MQTT broker separately (e.g., `mosquitto -c mosquitto.conf`)

4. Start the app with required env vars:
   ```bash
   cd backend && HTTP_HOST=0.0.0.0 HTTP_PORT=8000 uv run python -m src.main
   ```

### Verify the server is running

```bash
curl http://localhost:8000/health
```
Expected response: `{"status": "ok"}`

## Configuration

All config via environment variables (loaded from `.env` if present). Defined in `src/config.py`:

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `HTTP_HOST` | Yes | — | Server bind host |
| `HTTP_PORT` | Yes | — | Server bind port |
| `DATABASE_URL` | No | `sqlite+aiosqlite:///./data/database.db` | SQLAlchemy async URL |
| `MQTT_BROKER_HOST` | No | `localhost` | MQTT broker hostname |
| `MQTT_BROKER_PORT` | No | `1883` | MQTT broker port |
| `MQTT_TOPIC` | No | `isic/scan` | Topic to subscribe to |
| `MQTT_CLIENT_ID` | No | `isic-backend` | MQTT client identifier |
| `DEBUG` | No | `false` | Debug mode |

In Docker Compose, `DATABASE_URL` and `MQTT_BROKER_HOST` are overridden to use container paths/hostnames.

## REST API Endpoints

| Method | Path | Description | Request | Response |
|--------|------|-------------|---------|----------|
| `GET` | `/health` | Health check | — | `{"status": "ok"}` |
| `GET` | `/scans` | List scans (paginated) | `?limit=N&offset=N` (both **required**) | `list[ScanResponse]` |
| `GET` | `/scans/{scan_id}` | Get single scan | — | `ScanResponse \| null` |
| `PATCH` | `/isics/{isic_identifier}` | Update ISIC name fields | `{"first_name": "...", "last_name": "..."}` | `ISICResponse` |

**Schemas** (defined in `src/api/schemas.py`):
- `ScanResponse`: id, isic_id, isic_identifier, first_name, last_name, timestamp, created_at
- `ISICUpdateRequest`: optional first_name, optional last_name
- `ISICResponse`: id, isic_identifier, first_name, last_name, created_at

## Architecture

```
src/
├── main.py              # FastAPI app + lifespan (MQTT startup/shutdown)
├── config.py            # Pydantic Settings (env vars)
├── api/
│   ├── routes.py        # REST endpoint handlers
│   └── schemas.py       # Pydantic request/response models
├── database/
│   └── connection.py    # Async SQLAlchemy engine + session factory
├── models/
│   ├── base.py          # DeclarativeBase
│   ├── isic.py          # ISIC card model (isics table)
│   └── scan.py          # ISICScan model (isic_scans table)
├── mqtt/
│   ├── client.py        # MQTTClient: connection loop, auto-reconnect
│   └── handler.py       # Message parsing, validation, DB insert
└── services/
    └── scan_service.py  # Database operations (CRUD for ISIC + scans)
```

**Data flow:**
1. Hardware sends JSON `{"isic_identifier": "..."}` to MQTT topic `isic/scan`
2. `mqtt/client.py` receives message, calls `mqtt/handler.py`
3. Handler parses JSON, extracts identifier, calls `scan_service.create_scan_with_identifier()`
4. Service auto-creates ISIC record if new, then creates scan event
5. Frontend fetches scans via `GET /scans`

**Key patterns:**
- Layered: routes → services → models → database
- All database operations are async (SQLAlchemy + aiosqlite)
- MQTT client auto-reconnects with 5s delay on connection loss
- Invalid MQTT messages are silently rejected with logging (no crash)

## Database

**Models:**

`isics` table (ISIC cards):
- `id` (PK), `isic_identifier` (unique, indexed), `first_name` (nullable), `last_name` (nullable), `created_at` (UTC)

`isic_scans` table (scan events):
- `id` (PK), `isic_id` (FK → isics.id, indexed), `timestamp` (UTC), `created_at` (UTC)

**Relationship:** Many scans → one ISIC card (back_populates both ways)

### Migrations

Managed by Alembic. Migration files in `migrations/versions/`.

```bash
# Apply all pending migrations
alembic upgrade head

# Create new auto-generated migration
alembic revision --autogenerate -m "description"

# Check current migration state
alembic current

# Downgrade one step
alembic downgrade -1
```

Always run `alembic upgrade head` after pulling changes or modifying models.

## Testing

Tests use real Docker containers (testcontainers) — no mocks.

```bash
cd backend && uv run pytest
```

**Requirements:** Docker must be running (tests spin up a Mosquitto container).

**Test structure:**
- `tests/conftest.py` — fixtures: async DB engine (temp SQLite), MQTT container, FastAPI test client
- `tests/test_e2e_mqtt.py` — 11 end-to-end tests covering MQTT message processing, scan creation, ISIC linking, and API responses

**Fixtures:**
- `mqtt_container` (session) — Mosquitto Docker container, shared across all tests
- `db_engine` (per-test) — fresh temporary SQLite database with schema
- `db_session` (per-test) — database session with rollback cleanup
- `mqtt_client` (per-test) — connected MQTTClient instance
- `test_client` (per-test) — FastAPI AsyncClient with overridden DB dependency

## Code Quality

```bash
# Lint (ruff: line-length 88, Python 3.13, strict rules)
uv run ruff check --fix .

# Type check (mypy: strict mode, pydantic + sqlalchemy plugins)
uv run mypy .
```

**Style rules (from CLAUDE.md):**
- No `from __future__ import annotations`
- No `TYPE_CHECKING` patterns — use direct imports
- Imports only at top of file, never inside functions
- Forward references: string literals with `# type: ignore[name-defined]` and `# noqa: F821`

## Common Development Workflows

### Adding a new API endpoint

1. Define Pydantic schemas in `src/api/schemas.py`
2. Add service functions in `src/services/scan_service.py`
3. Add route handler in `src/api/routes.py`
4. Add tests in `tests/`
5. Run `uv run mypy .` and `uv run ruff check --fix .`
6. Run `uv run pytest`

### Adding a new database model/field

1. Modify or create model in `src/models/`
2. Import the model in `src/models/__init__.py` if new
3. Generate migration:
   ```bash
   alembic revision --autogenerate -m "add xyz to table"
   ```
4. Review the generated migration in `migrations/versions/`
5. Apply: `alembic upgrade head`
6. Update related services and schemas

### Debugging MQTT message handling

1. Check the app logs for `mqtt/handler.py` output (uses loguru)
2. Publish a test message manually:
   ```bash
   # If mosquitto_pub is installed locally:
   mosquitto_pub -h localhost -p 1883 -t "isic/scan" -m '{"isic_identifier": "TEST123"}'

   # Via the Docker MQTT container (always works):
   docker exec $(docker ps -qf "ancestor=eclipse-mosquitto:2.0") mosquitto_pub -h localhost -p 1883 -t "isic/scan" -m '{"isic_identifier": "TEST123"}'
   ```
3. Verify the scan was created:
   ```bash
   curl http://localhost:8000/scans
   ```
4. If the message is rejected, handler logs will show why (invalid JSON, missing identifier field)

### Running a single test

```bash
cd backend && uv run pytest tests/test_e2e_mqtt.py::test_name -v
```
