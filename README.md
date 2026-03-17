# ISIC Project

Student identification and attendance tracking system for STU Bratislava. Teachers manage semester schedules, track student attendance via a weekly calendar UI, and automate attendance recording through NFC card scanning.

## Architecture

```
┌─────────────┐     MQTT      ┌─────────────┐     REST API     ┌─────────────┐
│  Hardware    │──────────────>│   Backend    │<────────────────>│  Frontend   │
│  ESP8266 +   │  isic/scan    │  FastAPI +   │                  │  React 19 + │
│  PN532 NFC   │               │  SQLAlchemy  │                  │  Vite       │
└─────────────┘               └──────┬───────┘                  └─────────────┘
                                     │
                               ┌─────┴─────┐
                               │  SQLite +  │
                               │  Mosquitto │
                               └───────────┘
```

**Flow:** Hardware scans ISIC cards via NFC -> publishes JSON to MQTT topic `isic/scan` -> Backend receives, auto-creates ISIC records, stores scans -> Frontend displays data via REST API.

## Components

Each component is a git submodule:


| Component         | Directory        | Stack                                                |
| ----------------- | ---------------- | ---------------------------------------------------- |
| **Frontend**      | `frontend/`      | React 19, TypeScript, Vite, Tailwind CSS, shadcn/ui  |
| **Backend**       | `backend/`       | FastAPI, async SQLAlchemy, aiosqlite, MQTT (aiomqtt) |
| **Hardware**      | `hardware/`      | ESP8266/ESP32, C++17, PlatformIO, PN532 NFC reader   |
| **Documentation** | `documentation/` | LaTeX thesis source                                  |


## Getting Started

### Clone with submodules

```bash
git clone --recurse-submodules https://github.com/Team-project-ISIC-attendance/isic-project.git
cd isic-project
```

### Backend

```bash
cd backend
uv sync
alembic upgrade head
docker-compose up          # Starts backend + MQTT broker (ports 8000, 1883)
```

API docs available at [http://localhost:8000/docs](http://localhost:8000/docs)

### Frontend

```bash
cd frontend
npm install
npm run dev                # Starts Vite dev server at http://localhost:5173
```

### Hardware

```bash
cd hardware
pio run                    # Build for ESP8266
pio run -t upload          # Flash to device
```

## Development

This project uses Claude Code with a Ralph QA loop for autonomous execution:

```bash
.ralph/loop.sh all            # Run all specs sequentially
.ralph/loop.sh phase 1        # Run all phase 1 specs
.ralph/loop.sh build 1.1      # Build a single spec
```

Interactive skills: `/backend-dev`, `/frontend-dev`, `/security-scan`, `/git-workflow`

## Git Workflow

Git Flow model. All work in feature/fix branches, merged via PRs with `--no-ff`. Conventional commits (`feat:`, `fix:`, `docs:`, `chore:`). No direct commits to `main`.