---
name: audit-claude-md
description: Audit CLAUDE.md for attention zone optimization, length, and extraction opportunities. Produces prioritized recommendations.
allowed-tools: Read, Grep, Glob
---

Audit the project's CLAUDE.md for effectiveness based on LLM attention patterns.

## Attention Model

LLM attention follows a U-shaped curve across long context:

| Zone | Position | Attention | Best for |
|------|----------|-----------|----------|
| Primacy | First 20% | Strong | Critical identity, project overview |
| Dead zone | Middle 30-60% | Weakest | Extract reference content here |
| Recency | Last 20% | Strong | Behavioral rules (MUST/NEVER), reminders |

## Workflow

```
Audit Progress:
- [ ] Measure file length
- [ ] Parse section structure
- [ ] Classify sections (behavioral vs reference)
- [ ] Map to attention zones
- [ ] Identify misplacements
- [ ] Recommend extractions
- [ ] Output report
```

## Step 1: Measure Length

Read `CLAUDE.md` and count lines.

| Length | Assessment |
|--------|-----------|
| < 50 lines | Compact — good |
| 50-100 lines | Normal — review structure |
| 100-200 lines | Long — consider extraction |
| > 200 lines | Too long — extraction required |

## Step 2: Parse Sections

List every `##` heading with its line range and content type.

## Step 3: Classify Content

**Behavioral** (should stay inline — these affect agent behavior):
- Rules with NEVER, MUST, ALWAYS, DO NOT
- Guardrails and constraints
- Workflow requirements
- Git conventions

**Reference** (safe to extract to `docs/` or skills):
- Architecture descriptions
- Command reference lists
- Code style details beyond rules
- File structure documentation

## Step 4: Map to Attention Zones

Calculate zones based on total lines:
- Primacy: lines 1 to total*0.20
- Dead zone: lines total*0.30 to total*0.60
- Recency: lines total*0.80 to total

Flag any behavioral content in the dead zone as a misplacement.

## Step 5: Extraction Candidates

For reference sections in the dead zone:
- Keep a 3-5 line stub with pointer: "See `docs/{topic}.md` for details"
- Move full content to separate file
- Architecture and command details are prime candidates

## Step 6: Report

```
CLAUDE.MD AUDIT
===============

Length: {N} lines ({assessment})

Section Map:
  {line range} | {section name} | {behavioral|reference} | {zone}

Misplacements (behavioral in dead zone):
  - {section} at lines {N-M}: {recommendation}

Extraction Candidates:
  - {section} ({N} lines): Move to {target file}
    Stub: "{suggested stub text}"

Recommendations (priority order):
  1. {highest impact change}
  2. {next change}
  ...

Reminders Section:
  {EXISTS|MISSING} — Add critical rules reminder at EOF for recency bias
```
