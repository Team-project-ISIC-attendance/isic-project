---
name: phase-start
description: Execute all tasks in a phase autonomously. Delegates each task to a subagent for implementation, then verifies and commits.
argument-hint: <phase-number>
allowed-tools: Read, Write, Edit, Bash, Glob, Grep, Skill, Agent
---

Execute all tasks in Phase $1 from docs/EXECUTION_PLAN.md.

**Critical:** The primary agent orchestrates only. All implementation work is delegated to subagents via the Agent tool to preserve context window.

## Workflow

```
Phase Start Progress:
- [ ] Read context (CLAUDE.md, docs/EXECUTION_PLAN.md, specs)
- [ ] Git setup (create phase branch)
- [ ] For each task: delegate → review → verify → commit
- [ ] Phase completion summary
```

## Context

Read these files first:
- **CLAUDE.md** — Project conventions
- **docs/EXECUTION_PLAN.md** — Task index with phase structure and spec pointers
- **.claude/verification-config.json** — Verification commands and Figma design config
- **.claude/phase-state.json** — Current progress

## Git Setup

Before starting the phase:

```bash
# Create feature branch from main for the task
git checkout main
git pull origin main
```

## Task Execution Loop

For each task in Phase $1 (in order from docs/EXECUTION_PLAN.md):

### 1. Check if already complete

Read phase-state.json. If task status is `COMPLETE`, skip it.

### 2. Read the spec (primary agent)

Read `docs/{task-spec-file}.md` for full implementation details. Understand:
- Goal and dependencies
- Files to create/modify
- E2E tests required
- PR checklist criteria

### 3. Create task branch (primary agent)

```bash
git checkout -b {branch-from-execution-plan} main
```

### 4. Prepare delegation prompt (primary agent)

Build a comprehensive prompt for the subagent that includes:
- The full spec content (copy relevant sections inline)
- Working directory (e.g., `backend/` or `frontend/`)
- Code style rules from CLAUDE.md (e.g., no `__future__` annotations, no `TYPE_CHECKING`)
- Test-first requirement: write tests BEFORE implementation
- Files to create/modify from the spec
- Existing patterns to follow (describe any relevant patterns found in the codebase)

**For frontend tasks (Phase 3):** Add to the prompt:
- "All visual components must match the Figma design 1:1"
- "Use Figma MCP tools (`get_design_context`, `get_screenshot`) with this URL: https://www.figma.com/design/wYwJVRnlPhF9uplIawpl0i/ISIC-Attendance--Copy-?node-id=616-41377&t=mYni0sAk4toIPrXX-1"
- "Compare your implementation against the Figma screenshot before considering the task complete"

**For MQTT/hardware tasks (Tasks 4.1, 4.2):** Add to the prompt:
- "No real NFC hardware available. Use mock MQTT messages for testing"
- "Publish test JSON to MQTT topic `isic/scan` to simulate card scans"
- "The mock approach must be swappable — real hardware should work with zero code changes"

### 5. Delegate to subagent (primary agent)

Spawn a subagent using the Agent tool:

```
Agent(
  description: "Implement task {id}: {short description}",
  prompt: {prepared prompt from step 4},
  subagent_type: "general-purpose"
)
```

The subagent will:
- Explore the codebase for existing patterns
- Write tests first (E2E tests from spec)
- Implement minimum code to pass tests
- Iterate until tests pass
- Return a summary of what was done

### 6. Review subagent output (primary agent)

After the subagent completes:
- Read the summary of changes
- Quick sanity check: files created match spec expectations
- Run lint/typecheck to catch obvious issues

### 7. Verify (primary agent)

Run `/verify-task {task-id}` to check all acceptance criteria from the spec.

If verification fails:
- Send follow-up to the same subagent with the failure details
- Or spawn a new subagent focused on fixing the specific failures
- Re-verify after fixes

### 8. Commit (primary agent)

```bash
git add {specific files}
git commit -m "feat(task-{id}): {description}"
```

Commit message format:
- `feat(task-1.1): add database models and migration`
- `feat(task-2.1): add semester and schedule API`

### 9. Update state (primary agent)

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
- Do implementation work directly (always delegate)

## Browser E2E Verification

**After every frontend or fullstack task (Phase 3 and 4)**, the primary agent must verify the app end-to-end in a real browser using Chrome DevTools MCP:

1. Start the backend (`cd backend && docker-compose up -d`)
2. Start the frontend (`cd frontend && npm run dev`)
3. Use Chrome DevTools MCP tools to:
   - Navigate to the app (`navigate_page`)
   - Verify the implemented feature works as specified (`take_screenshot`, `click`, `fill`, `evaluate_script`)
   - Compare visual output against the Figma design
   - Check for console errors (`list_console_messages`)
4. Document the browser verification result in the task report

This is mandatory — do not skip browser verification for any task that has UI changes. The app must work end-to-end in the real browser, not just pass unit tests and build checks.

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
