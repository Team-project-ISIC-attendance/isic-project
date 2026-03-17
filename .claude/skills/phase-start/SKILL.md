---
name: phase-start
description: Execute all tasks in a phase autonomously. Implements test-first, then code, then verify, then commit per task.
argument-hint: <phase-number>
allowed-tools: Read, Write, Edit, Bash, Glob, Grep, Skill
---

Execute all tasks in Phase $1 from docs/EXECUTION_PLAN.md.

## Workflow

```
Phase Start Progress:
- [ ] Read context (CLAUDE.md, docs/EXECUTION_PLAN.md, specs)
- [ ] Git setup (create phase branch)
- [ ] Execute tasks sequentially
- [ ] Phase completion summary
```

## Context

Read these files first:
- **CLAUDE.md** — Project conventions
- **docs/EXECUTION_PLAN.md** — Task index with phase structure and spec pointers
- **docs/** — Detailed spec for each task
- **.claude/verification-config.json** — Verification commands
- **.claude/phase-state.json** — Current progress

## Git Setup

Before starting the phase:

```bash
# Commit any dirty files (preserves work)
git add -A && git diff --cached --quiet || git commit -m "wip: uncommitted changes before phase-$1"

# Create phase branch
git checkout -b phase-$1
```

If `phase-$1` branch already exists (resuming), just check it out:
```bash
git checkout phase-$1
```

## Task Execution Loop

For each task in Phase $1 (in order from docs/EXECUTION_PLAN.md):

### 1. Check if already complete

Read phase-state.json. If task status is `COMPLETE`, skip it.

### 2. Read the spec

Read `docs/{task-spec-file}.md` for full implementation details.

### 3. Explore before implementing

- Search for similar existing functionality
- Identify patterns used elsewhere in codebase
- List reusable utilities/components
- Note naming conventions and error handling patterns

### 4. Write tests first

Write E2E tests based on the spec's E2E Tests section and acceptance criteria.
Each acceptance criterion should map to at least one test.

### 5. Implement

Write minimum code to pass tests, following discovered patterns.
Use the files listed in the spec's "Files to Create/Modify" section.

### 6. Verify

Run `/verify-task {task-id}` to check all acceptance criteria.

### 7. Commit

```bash
git add -A
git commit -m "feat(task-{id}): {description}"
```

Commit message format:
- `feat(task-1.1): add database models and migration`
- `feat(task-2.1): add semester and schedule API`

### 8. Self-Reflection

After completing the task, answer the self-reflection questions from the spec.
Log the answers briefly as part of the commit or in a comment.

### 9. Update state

Update `.claude/phase-state.json`:
```json
{
  "tasks": {
    "{task-id}": {
      "status": "COMPLETE",
      "completed_at": "{ISO timestamp}"
    }
  }
}
```

## Stuck Detection

Track failures in `.claude/phase-state.json`.

**Escalate to human when:**
- 3 consecutive task failures
- Same error appears 2+ times
- 5 verification attempts on same criterion

**When stuck, report:**
```
STUCK: Phase $1, Task {id}

Pattern: {what keeps failing}
Attempts: {N}

Last errors:
1. {error}
2. {error}

Options:
1. Skip this task and continue
2. Modify acceptance criteria
3. Different approach: {suggestion}
4. Abort phase
```

Do NOT:
- Keep retrying the same approach
- Silently skip failing tasks
- Reduce test coverage to make things pass

## Completion

When all tasks in Phase $1 are done:

```
PHASE $1 COMPLETE
=================
Tasks completed: {N}/{N}
Branch: phase-$1
Commits: {list}

Files created/modified:
- {file list}

Next: Run /go to advance to checkpoint or next phase
```

Update phase-state.json:
```json
{
  "phases": {
    "$1": { "status": "COMPLETE" }
  }
}
```
