---
name: audit-skills
description: Audit all skills in .claude/skills/ against best practices. Scores and prioritizes findings.
allowed-tools: Read, Grep, Glob
---

Audit all skills for quality, completeness, and adherence to best practices.

## Workflow

```
Audit Skills Progress:
- [ ] Discover auditable skill files
- [ ] Audit each skill against criteria
- [ ] Score and prioritize findings
- [ ] Output report
```

## Step 1: Discover Skills

Find all `SKILL.md` files under `.claude/skills/`:
```
.claude/skills/*/SKILL.md
```

## Step 2: Audit Criteria

For each skill file, check:

### Length & Structure (L)
- **L1** (Critical): Under 500 lines? Skills over 500 lines lose coherence.
- **L2** (Medium): Has frontmatter (name, description, allowed-tools)?
- **L3** (Medium): Description under 200 chars and action-oriented?

### Checklists & Verification (C)
- **C1** (Critical): Has a progress tracking checklist? (```\n- [ ] Step...\n```)
- **C2** (Critical): Has clear exit conditions (success + failure)?
- **C3** (Medium): References verification commands or other skills for validation?

### Step Clarity (S)
- **S1** (Medium): Each step has a clear action (not just "handle" or "process")?
- **S2** (Low): Steps are numbered or ordered?
- **S3** (Medium): No ambiguous branching (every if/else has explicit actions)?

### Error Handling (E)
- **E1** (Medium): Documents what to do when things fail?
- **E2** (Low): Has an error handling table or section?

### Description Quality (D)
- **D1** (Medium): Description tells WHEN to use the skill, not just what it does?
- **D2** (Low): argument-hint provided if skill takes arguments?
- **D3** (Medium): Description distinguishes this skill from similar ones?

## Step 3: Scoring

**Severity levels:**
- Critical (blocks execution): L1, C1, C2
- Medium (degrades quality): L2, L3, C3, S1, S3, E1, D1, D3
- Low (polish): S2, E2, D2

**Prioritization:**
1. Most violations in a single skill
2. Most common violation across skills
3. Critical severity first

## Step 4: Report

```
SKILLS AUDIT
============

Skills found: {N}

Per-Skill Results:
─────────────────
{skill-name} ({N} lines)
  PASS: {list}
  FAIL: {list with severity}
  Score: {passed}/{total} ({percentage}%)

Summary:
  Total checks: {N}
  Passed: {X}
  Failed: {Y}

Top Issues (by impact):
  1. [{severity}] {criterion}: {description} — affects {skill list}
  2. [{severity}] {criterion}: {description} — affects {skill list}

Recommendations:
  1. {highest priority fix}
  2. {next fix}
```
