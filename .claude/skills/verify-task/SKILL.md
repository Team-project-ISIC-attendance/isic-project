---
name: verify-task
description: Verify a single task's acceptance criteria from its spec file. Use after implementing a task.
argument-hint: <task-id e.g. "1.1" or "2.3">
allowed-tools: Read, Edit, Bash, Grep, Glob
---

Verify Task $1 against its acceptance criteria from the task spec file.

## Workflow

```
Verify Task Progress:
- [ ] Locate spec file via docs/EXECUTION_PLAN.md
- [ ] Parse criteria from spec
- [ ] Pre-flight check
- [ ] Verify each criterion
- [ ] Handle failures
- [ ] Generate report
- [ ] Update state
```

## Step 1: Locate Spec File

1. Read `docs/EXECUTION_PLAN.md`, find the row for Task $1
2. Extract the spec file path (e.g., `docs/task-01-database-models.md`)
3. Extract the directory (e.g., `backend/` or `frontend/`)

## Step 2: Parse Criteria

1. Read the spec file found in Step 1
2. Extract acceptance criteria from these sections (in order):
   - **E2E Tests** — each bullet is a testable criterion
   - **PR Checklist** — each `- [ ]` item is a criterion
3. For each criterion, infer the verification type from its text:
   - Contains `pytest`, `test` → **TEST**
   - Contains `mypy` → **TYPE**
   - Contains `ruff`, `lint`, `eslint` → **LINT**
   - Contains `build`, `docker`, `npm run build` → **BUILD**
   - Contains file path or `no changes to` → **CODE**
   - Contains `browser`, `visual` → **BROWSER**
   - Otherwise → **MANUAL**

Read `.claude/verification-config.json` to get the correct commands for the task's directory.

## Step 3: Pre-flight Check

Confirm each criterion is testable:
- Required files exist
- Verification commands available
- For TEST type: test runner works (`cd {directory} && uv run pytest --co -q` or `npm test -- --listTests`)

## Step 4: Verify Each Criterion

For each criterion, run verification by inferred type:

### TEST
Determine the directory from the task (backend = `backend/`, frontend = `frontend/`).

```bash
cd {directory} && {test command}
```

If the criterion references a specific test, run only that test. Otherwise run the full suite.

### LINT
```bash
cd {directory} && {lint command from verification-config.json}
```

### TYPE
```bash
cd {directory} && {typecheck command from verification-config.json}
```

### BUILD
```bash
cd {directory} && {build command}
```

### CODE
Read the referenced file and check the stated condition. Look for the specific code pattern or content described.

### BROWSER
Use Chrome DevTools MCP tools if available. If not, mark as MANUAL with instructions for manual verification.

**Record each result:**
```
[V-{N}] {PASS|FAIL|BLOCKED}
  Criterion: {text}
  Evidence: {command output or file reference}
  Fix: {if FAIL, suggested fix}
```

### Failure Handling

On FAIL:
1. Attempt to fix (up to 3 attempts per criterion)
2. Re-verify after fix
3. If 3 attempts exhausted, mark as FAIL and continue to next criterion

## Step 5: Report

```
TASK VERIFICATION: $1
=====================

Spec: {spec file path}
Directory: {directory}
Criteria: {total}
Passed: {X}
Failed: {Y}
Blocked: {Z}

Details:
[V-001] PASS — {summary}
[V-002] FAIL — {summary}
  Attempts: 3
  Blocker: {description}
```

## Step 6: Update State

### On Success (all criteria pass)

1. Update `.claude/phase-state.json`:
   ```json
   {
     "tasks": {
       "$1": {
         "status": "COMPLETE",
         "completed_at": "{ISO timestamp}"
       }
     }
   }
   ```
2. Report: "Task $1 verified. All criteria met."

### On Failure

1. Update phase-state.json with failure info:
   ```json
   {
     "tasks": {
       "$1": {
         "status": "IN_PROGRESS",
         "failures": {
           "consecutive": {N},
           "last_errors": ["{error}"]
         }
       }
     }
   }
   ```
2. Report which criteria failed with fix recommendations
