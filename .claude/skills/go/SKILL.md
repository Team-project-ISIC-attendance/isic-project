---
name: go
description: Resume execution from wherever you left off. Detects current state and runs the appropriate next skill. Use at the start of any session.
allowed-tools: Read, Glob, Grep, Bash, Skill
---

Determine where execution stands and either continue or report blockers.

## Workflow

```
Go Progress:
- [ ] Read phase state
- [ ] Read docs/EXECUTION_PLAN.md
- [ ] Determine next action
- [ ] Execute or report blockers
```

## Step 1: Load State

1. Read `.claude/phase-state.json`
2. Read `docs/EXECUTION_PLAN.md` — count `## Phase N:` headers for TOTAL_PHASES
3. Read `.claude/verification-config.json`

If `phase-state.json` doesn't exist or is invalid, report and suggest running `/phase-start 1`.

## Step 2: Determine Next Action

Walk through these conditions in order. Take the FIRST match:

### Case A: No Phase State

**Condition:** `phase-state.json` doesn't exist or has no valid `main` key.

**Action:** Report that no prior state exists. Read `docs/EXECUTION_PLAN.md`, summarize the plan, and invoke `/phase-start 1`.

### Case B: Phase In Progress

**Condition:** Current phase status is `IN_PROGRESS`. Unchecked `- [ ]` criteria exist for that phase.

**Action:** Invoke `/phase-start {CURRENT_PHASE}`. Phase-start handles resumption.

```
RESUMING EXECUTION
==================
Phase {CURRENT_PHASE} in progress. Resuming...
```

### Case C: Phase Complete — Needs Verification

**Condition:** ALL task criteria for current phase are checked `[x]`. Phase status is NOT `CHECKPOINTED`.

**Action:** Run verification commands from `.claude/verification-config.json` for the relevant component (backend/frontend). Report results and update phase status to `CHECKPOINTED`.

```
RUNNING CHECKPOINT
==================
All Phase {CURRENT_PHASE} tasks complete. Running quality gates...
```

Run backend verification:
```bash
cd backend && uv run pytest && uv run ruff check . && uv run mypy .
```

Run frontend verification (if Phase 3+):
```bash
cd frontend && npm run build && npm run lint
```

### Case D: Phase Checkpointed — More Phases

**Condition:** Current phase is `CHECKPOINTED`. CURRENT_PHASE < TOTAL_PHASES.

**Action:** Advance to next phase. Update phase-state.json and invoke `/phase-start {CURRENT_PHASE + 1}`.

```
ADVANCING TO NEXT PHASE
========================
Phase {CURRENT_PHASE} checkpointed. Starting Phase {CURRENT_PHASE + 1}...
```

### Case E: All Phases Complete

**Condition:** CURRENT_PHASE >= TOTAL_PHASES and status is `CHECKPOINTED` or `COMPLETE`.

```
EXECUTION COMPLETE
==================
All {TOTAL_PHASES} phases finished.

Summary:
- Phases completed: {TOTAL_PHASES}/{TOTAL_PHASES}
- Total tasks: {count from docs/EXECUTION_PLAN.md}
```

### Case F: Blocked

**Condition:** A task has status `BLOCKED` or 3+ consecutive failures.

```
EXECUTION BLOCKED
=================
Phase {CURRENT_PHASE} is blocked.

Task {id}: {description}
  Consecutive failures: {count}
  Last errors:
    - {error 1}
    - {error 2}

Options:
  1. Fix the issue and run /go again
  2. /phase-start {CURRENT_PHASE} — Retry from current task
  3. Edit docs/EXECUTION_PLAN.md — Modify blocked criteria
```
