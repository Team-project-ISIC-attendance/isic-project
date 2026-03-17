---
name: security-scan
description: Run dependency audits, secrets detection, and static analysis on the project.
allowed-tools: Read, Bash, Grep, Glob
---

Run security checks across the ISIC project.

## Workflow

```
Security Scan Progress:
- [ ] Dependency audit
- [ ] Secrets detection
- [ ] Static analysis
- [ ] Report findings
```

## Step 1: Dependency Audit

### Backend
```bash
cd backend && uv run pip-audit 2>/dev/null || echo "pip-audit not installed — skipping"
```

### Frontend
```bash
cd frontend && npm audit 2>/dev/null || echo "npm audit skipped"
```

## Step 2: Secrets Detection

Scan for accidentally committed secrets:

```
Patterns to check:
- AWS keys: AKIA[0-9A-Z]{16}
- GitHub tokens: gh[ps]_[A-Za-z0-9_]{36,}
- Generic API keys: (api[_-]?key|apikey)\s*[:=]\s*['"][^'"]{10,}
- Private keys: -----BEGIN (RSA |EC |DSA )?PRIVATE KEY-----
- JWT secrets: (jwt[_-]?secret|JWT_SECRET)\s*[:=]\s*['"][^'"]{8,}
- Database URLs with passwords: (mysql|postgres|mongodb)://[^:]+:[^@]+@
- .env files with secrets: (SECRET|PASSWORD|TOKEN|KEY)\s*=\s*[^\s]{8,}
```

Search in all tracked files:
```bash
git ls-files | xargs grep -l -E "AKIA[0-9A-Z]{16}|gh[ps]_[A-Za-z0-9_]{36,}|-----BEGIN.*PRIVATE KEY-----|password\s*=\s*['\"][^'\"]{8,}" 2>/dev/null
```

Check .gitignore covers sensitive files:
- `.env` (not `.env.example`)
- `*.pem`, `*.key`
- `data/*.db` (SQLite databases)

## Step 3: Static Analysis

### Backend
```bash
cd backend && uv run ruff check . --select S 2>/dev/null || echo "Security rules check skipped"
```

Check for common issues:
- SQL injection (raw SQL without parameterization)
- Hardcoded credentials in source
- Missing input validation on API endpoints
- CORS set to `*` in production config

### Frontend
- Check for `dangerouslySetInnerHTML` usage
- XSS risks in user-rendered content
- Sensitive data in localStorage (only JWT token is acceptable)

## Step 4: Report

```
SECURITY SCAN REPORT
====================

Dependencies:
  Backend: {N} vulnerabilities ({critical}/{high}/{medium}/{low})
  Frontend: {N} vulnerabilities

Secrets:
  {N} potential secrets found
  {details or "None detected"}

Static Analysis:
  {N} security issues found
  {details or "Clean"}

.gitignore Coverage:
  {PASS|FAIL} — {details}

Severity: CRITICAL | HIGH | MEDIUM | LOW | CLEAN
```

### Severity Definitions

- **CRITICAL**: Exposed secrets in tracked files, exploitable vulnerabilities
- **HIGH**: Missing .gitignore for sensitive files, known CVEs in dependencies
- **MEDIUM**: Overly permissive CORS, hardcoded non-secret config
- **LOW**: Informational findings, best practice suggestions
