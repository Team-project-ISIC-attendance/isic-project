---
name: git-workflow
description: Git workflow conventions for the ISIC Project. Use when creating branches, committing, merging, tagging releases, or any git operations. Codifies git flow rules, branch naming, critical branch policies, commit standards, release tagging, and the Claude Web workaround. Consult this skill before any git operation to ensure compliance with project conventions.
---

# Git Workflow Management

## Quick Reference

**CRITICAL - NEVER do these:**
- NEVER rebase `master` or `develop`
- NEVER force push to `master` or `develop`
- NEVER push/merge to `master` without explicit user approval
- NEVER commit directly on `develop` -- all work in feature/fix branches
- NEVER merge `master` into `develop` -- only fast-forward (`--ff-only`) after releases/hotfixes
- NEVER create commits that leave the project in a broken state
- NEVER use `git push --force` -- always use `git push --force-with-lease` instead
- NEVER merge without `--no-ff` flag (except develop fast-forward after release)
- NEVER bundle unrelated concerns into a single commit
- NEVER include AI attribution in commit messages (no "made with Claude", no "Co-Authored-By", etc.)
- NEVER merge into `develop` or `master` without a Pull Request
- NEVER release without creating a GitHub Release after tagging

**Branch naming:**
- Features: `feature/<description>`
- Fixes: `fix/<description>`
- Refactoring: `refactor/<description>`
- Documentation: `docs/<description>`
- Claude web only: `claude/<description>-<session-id>`

**All merges use `--no-ff`:**
```bash
git merge --no-ff <branch>
```

**Commit message format** (conventional commits):
```
<type>: <concise description>
```
Types: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`

## Branch Model (Git Flow)

```
master (production, auto-deploys)
  |
  +-- develop (integration)
        |
        +-- feature/*, fix/*, refactor/*, docs/* (work branches)
```

- `master` = production. Auto-deploys on push. There are NO GitHub branch protection rules -- any push goes straight to production, which makes carelessness extremely dangerous.
- `develop` = integration branch. All work merges here first.
- Work branches (feature/fix/refactor/docs) = short-lived, branch from `develop`, merge back into `develop`.
- No release branches. `develop` merges directly into `master` when ready.

### Repos Without a develop Branch

Some repositories (e.g., `isic-project` aggregator) only have `main` and no `develop` branch. In these cases:
- Branch work branches off `main` instead of `develop`
- PRs target `main` instead of `develop`
- The release/hotfix process does not apply (no two-tier branch model)
- All other rules still apply: `--no-ff` merges, branch cleanup, conventional commits, assignees, labels

## Critical Branches (NOT GitHub-Protected -- Handle With Extreme Care)

**WARNING:** Neither `master` nor `develop` have GitHub branch protection rules. There is no safety net -- any push goes through immediately. This means discipline is the ONLY thing preventing accidental production deployments or history corruption.

### master

- **NEVER** push directly without explicit user approval -- pushes auto-deploy to production
- **NEVER** rebase
- **NEVER** force push
- Only receives `--no-ff` merges from `develop` (releases) or direct hotfix commits (emergencies only, with explicit approval)
- Every commit on master MUST be tagged with a version (merge commits and hotfixes alike)

### develop

- **NEVER** rebase
- **NEVER** force push
- **NEVER** commit directly on `develop` -- all work happens in feature/fix branches
- **NEVER** merge `master` into `develop` -- only fast-forward is allowed after a release (see Releases section)
- Receives `--no-ff` merges from feature/fix branches

## Commit Standards

### Message Format

```
<type>: <concise title>

<optional body -- why, context, details>
```

The title is always required. The body is optional but usually warranted -- use it when the "why" or context is not obvious from the title alone. Separate the title from the body with a blank line.

**When to include a body:**
- The change has a non-obvious reason or motivation
- There is important context (what was tried, what was considered, what tradeoff was made)
- The change affects behavior in a way that is not self-evident from the diff
- Multiple files are touched and the title alone does not convey the scope

**When a title alone is enough:**
- Trivial changes where the title says it all (e.g., `docs: fix typo in README`)
- Single-file changes with an obvious purpose

**Types:**
| Type | Use for |
|------|---------|
| `feat` | New features or capabilities |
| `fix` | Bug fixes |
| `refactor` | Code restructuring without behavior change |
| `test` | Adding or updating tests |
| `docs` | Documentation changes |
| `chore` | Maintenance, dependency updates, .gitignore, etc. |

**Rules:**
- No filler words, no noise
- No AI attribution in commit messages -- no "made with Claude", no "Co-Authored-By", no "generated by AI"
- Concise and descriptive -- say what changed and why if not obvious
- Lowercase after the type prefix
- **No "and" in commit titles** -- if you need "and", the commit likely bundles two concerns and should be split into separate commits
- Use backticks around code references (class names, function names, variables) for nice GitHub rendering -- e.g., `` fix: handle nullable FK in `DependencyAnalyzer` ``

**Shell escaping for backticks:** When committing from the terminal, backticks in commit messages must be escaped or the message must use single quotes to avoid shell interpretation:

```bash
# Option 1: Use single quotes for the whole message (backticks are literal inside single quotes)
git commit -m 'fix: handle nullable FK in `DependencyAnalyzer`'

# Option 2: Escape backticks with backslash inside double quotes
git commit -m "fix: handle nullable FK in \`DependencyAnalyzer\`"

# Option 3: Use heredoc (most reliable for complex messages with body)
git commit -m "$(cat <<'EOF'
fix: handle nullable FK in `DependencyAnalyzer`

Nullable foreign keys were treated as mandatory dependencies,
causing false cycle detection between unrelated tables.
EOF
)"
```

**Examples:**

Title only (trivial):
```
docs: fix typo in README
chore: remove seeding plans for Docker database
```

Title + body (typical):
```
feat: implement cost polling service for LangSmith trace costs

Polls LangSmith API for trace costs after seeding completes.
Results are submitted to the backend as part of seed metadata.

fix: enforce positive integer constraint for `records_count`

The scoping graph could produce zero or negative counts when
the user scope description was ambiguous. Added validation
at the graph output boundary.

refactor: extract `TokenBucket` from rate limiter

Separates bucket logic from the priority queue so each can
be tested independently. No behavior change.
```

### Each Commit = Working Snapshot

- Every commit MUST leave the project in a working state (builds, passes lint/type checks)
- Before committing, verify that linting (`uv run ruff check .`) and type checking (`uv run mypy .`) pass
- For teammates: the teammate making changes is responsible for running these checks. The team lead should verify by asking them before approving commits.

### Separate Concerns = Separate Commits

- Each logically distinct change gets its own commit
- A bug fix and a refactoring discovered during the fix are TWO commits, not one
- Do not bundle unrelated changes
- **Tooling files are separate from code changes.** Skills (`.claude/skills/`, `.cursor/skills/`), agent configs (`.claude/agents/`), and `CLAUDE.md` files are committed separately from application code -- even if they were created/modified as part of the same task. Use `docs:` or `chore:` prefix for these.
- **`.claude/skills/` changes are committed together.** When multiple skill files change as part of one concern, commit them together.
- **When multiple concerns exist in the working directory, consult the user with clear options before committing.** Present a breakdown of which changes go into which commit so the user can confirm or adjust.

**Example -- correct way to consult:**
```
The working directory has changes across 4 files. I see two logical concerns:

Commit 1 - "fix: handle nullable FK in dependency resolver"
  - src/ai/coordination/dependency_analyzer.py (lines 45-60: null check)
  - tests/test_dependency_analyzer.py (new test case)

Commit 2 - "refactor: simplify topological sort loop"
  - src/ai/coordination/dependency_analyzer.py (lines 80-95: loop rewrite)
  - src/ai/coordination/seeding_orchestrator.py (updated call site)

Does this split look right, or would you like a different grouping?
```

- Always list files and the specific changes per proposed commit
- The user confirms or adjusts before you stage and commit

### Separate Concerns = Separate Branches

- Ideally, each distinct concern (feature, fix, refactoring) should live in its own branch
- **When in doubt, consult the user with clear options.** Do not just ask an open-ended question. Present structured options so the user can pick one or propose an alternative.

**Example -- correct way to consult:**
```
I see three logical changes in the working directory:
1. Bug fix for validation edge case
2. Refactoring of validation logic
3. New test coverage for validation

Options:
A) Three separate branches: fix/validation-edge-case, refactor/validation-logic, test/validation-coverage
B) One branch with three separate commits (fix, refactor, test)
C) Two branches: fix/validation-edge-case (fix + test), refactor/validation-logic

Which do you prefer, or would you like a different split?
```

- Always list the concrete changes, then present numbered/lettered options
- The user decides the granularity -- do not assume

### Partial File Staging (`git add -p`)

When a single file contains changes belonging to different logical concerns, use **patch staging** to craft precise commits:

```bash
# Interactive: select specific hunks within a file
git add -p <file>

# Then commit only the staged hunks
git commit -m "fix: address validation edge case"

# Stage remaining hunks for the next commit
git add -p <file>
git commit -m "refactor: simplify validation logic"
```

This is essential when the working directory has accumulated changes across multiple concerns.

**CRITICAL: Working state verification with partial staging.** When staging only parts of files, the resulting commit may not compile or work because related changes were left unstaged. You MUST verify each partially-staged commit is a working snapshot:

1. Stage the hunks for one concern
2. Stash ONLY the tracked unstaged changes (see Stash Safety below): `git stash push --keep-index`
3. Run verification: `uv run ruff check .` and `uv run mypy .`
4. If passing, commit
5. Pop stash immediately: `git stash pop`
6. Repeat for next concern

**Note for Claude Code agents:** The Bash tool does not support interactive mode, so `git add -p` cannot be used directly. Instead:
- Use `git add <specific-files>` when entire files belong to one concern
- When a file has mixed changes, consider whether the file can be committed whole (if all changes are related) or flag to the user that manual `git add -p` is needed

### Stash Safety

**CRITICAL: This repo always has many untracked and uncommitted files in the working directory. This is normal and intentional -- not everything belongs in git.** Do NOT assume the working directory is clean or that your changes are the only changes present.

**The user may have existing stashes that have been there for a long time.** These are the user's work and must NEVER be touched, dropped, popped, or modified unless the user explicitly asks. When you create a temporary stash, you are responsible for managing ONLY your own stash entry -- leave everything else untouched.

**NEVER do these with stashes:**
- NEVER run bare `git stash` -- it stashes ALL tracked modifications, not just yours
- NEVER run `git stash -u` or `git stash --include-untracked` -- this captures untracked files too, which can destroy the user's work-in-progress files that are intentionally not in git
- NEVER run `git stash drop` or `git stash clear` -- the user's stashes are off-limits
- NEVER run `git stash pop` on a stash that is not yours (always verify with `git stash list` that you are popping `stash@{0}` which is the one you just created)
- NEVER assume your changes are the only changes in the repo

**Safe stash patterns:**

```bash
# SAFE: Stash only specific files you are working with
git stash push -- src/ai/graphs/file1.py src/ai/tools/file2.py

# SAFE: Stash only tracked changes, keep staged intact (for partial commit verification)
git stash push --keep-index

# SAFE: Always pop immediately after temporary use
git stash pop

# SAFE: Check what's in a stash before doing anything with it
git stash show -p stash@{0}

# SAFE: List all stashes to understand what exists
git stash list
```

**Workflow for temporary stashing:**

1. **Before stashing:** Run `git stash list` and note the count and top entry -- these are the user's stashes, do NOT touch them
2. **Stash narrowly:** Use `git stash push -- <specific-files>` to stash ONLY the files you need
3. **Do your temporary work** (verification, branch switch, etc.)
4. **Pop immediately:** `git stash pop` as soon as you are done -- this pops `stash@{0}` which should be the one you just created
5. **Verify:** Run `git stash list` again and confirm the count and entries match what you noted in step 1 (all user stashes intact, yours is gone)

If you need to stash for partial commit verification, use `git stash push --keep-index` which only stashes unstaged tracked changes and leaves staged changes in place. Pop it immediately after verification.

## Standard Workflow

### 1. Create a Feature Branch

```bash
git checkout develop
git pull origin develop
git checkout -b feature/my-feature
```

### 2. Work and Commit

```bash
# Stage specific files (never use git add -A blindly)
git add src/ai/graphs/my_file.py
git commit -m "feat: add new graph node for validation"
```

### 3. Keep Up to Date (Before Push Only)

If the branch has NOT been pushed yet, pull develop first then rebase onto local develop:

```bash
git checkout develop
git pull origin develop
git checkout <feature-branch>
git rebase develop
```

**NEVER rebase directly onto `origin/develop` without pulling first.** Always update the local `develop` branch so it is in sync with the remote before rebasing — this ensures any local develop state is current and the rebase target is unambiguous.

If already pushed, merge instead:

```bash
git checkout develop
git pull origin develop
git checkout <feature-branch>
git merge develop
```

### 4. Push Feature Branch

```bash
git push -u origin feature/my-feature
```

### 5. Create Pull Request (MANDATORY)

**A PR is required before any merge into develop.** No exceptions.

```bash
gh pr create --base develop --title '<concise title>' --body "$(cat <<'EOF'
## Summary

<What changed and why>

<... additional sections as relevant ...>
EOF
)"
```

See the Pull Requests section for writing principles.

### 6. Merge into Develop

After PR is created, reviewed, and CI passes (see "Before Merging Any PR"):

```bash
git checkout develop
git pull origin develop
git merge --no-ff feature/my-feature
git push origin develop
```

### 7. Clean Up (MANDATORY -- ALWAYS do this immediately after pushing develop)

**Branch cleanup is not optional.** Every merge must be followed by deleting the branch -- both locally and on the remote. Do not skip this step. Do not defer it. Do not move on to the next task without completing it.

```bash
git branch -d feature/my-feature
git push origin --delete feature/my-feature
git fetch origin --prune
```

The `git fetch --prune` removes any stale remote-tracking refs that remain after the remote branch is deleted.

Clean up merged branches by default. Stale branches clutter `git branch` output and make it harder to identify active work. Skip cleanup only when there is a clear reason to keep the branch temporarily (e.g., a hotfix branch referenced in an ongoing incident).

#### Bulk Cleanup of Already-Accumulated Branches

If branches have already accumulated from previous merges, clean them up before starting new work.

**Find local branches whose upstream is gone** (already deleted on remote):

```bash
git fetch origin --prune
git branch -vv | grep ': gone]'
```

**Delete them safely** (one at a time or in bulk):

```bash
# Delete a single branch
git branch -d <branch-name>

# Bulk delete all branches whose upstream is gone (review output of grep first)
git branch -vv | grep ': gone]' | awk '{print $1}' | xargs git branch -d
```

Use `-d` (safe delete, refuses if unmerged) rather than `-D` (force delete). If `-d` refuses, the branch has unmerged work -- investigate before forcing.

## Releases

### Merging develop into master

**Requires explicit user approval. Follow this exact sequence -- every step is mandatory.**

**Step 1: Prepare**

```bash
git checkout develop
git pull origin develop

git checkout master
git pull origin master
```

Ensure no conflicts on either branch.

**Step 2: Create Release PR (MANDATORY)**

A PR from develop to master is required before merging. Title must follow the `release: vX.Y.Z` format.

```bash
gh pr create --base master --head develop --assignee @me --title 'release: vX.Y.Z' --body "$(cat <<'EOF'
# Human-Readable Release Title

## Summary

<What this release contains and why it matters>

<... additional sections as relevant ...>
EOF
)"
```

**Step 3: Verify CI passes and merge locally (DO NOT PUSH YET)**

Verify CI passes on the release PR (see "Before Merging Any PR"), then:

```bash
git checkout master
git merge --no-ff develop
```

**Step 4: STOP and ask the user for confirmation**

Show the user what will be pushed (commit log, tag name). Wait for explicit approval.

**Step 5: After approval -- push master with tag**

```bash
git tag release/v<version>
git push origin master --tags
```

**Step 6: Fast-forward develop onto the new master merge commit**

```bash
git checkout develop
git merge --ff-only master
git push origin develop
```

**Step 7: Create GitHub Release (MANDATORY)**

```bash
gh release create release/v<version> --title 'vX.Y.Z' --notes "$(cat <<'EOF'
<Same content as the release PR body>
EOF
)"
```

**CRITICAL:** NEVER merge master back into develop with a merge commit. Only `--ff-only` is allowed here. After a release, develop must point to the same commit as master (the merge commit). If `--ff-only` fails, something is wrong -- do NOT force it, investigate.

**Tag format:** `release/vX.Y.Z` (semver with `v` prefix)

### Version Incrementing Rules

Given the current version `release/vX.Y.Z`:

| Increment | When | Who decides |
|-----------|------|-------------|
| `Z` (patch: `1.0.0` -> `1.0.1`) | Bug fixes, hotfixes | Agent can auto-increment |
| `Y` (minor: `1.0.1` -> `1.1.0`) | New features | Agent can auto-increment |
| `X` (major: `1.1.0` -> `2.0.0`) | Breaking changes, major milestones | **User approval REQUIRED** -- never auto-increment |

**How to determine the next version:**
1. Check the latest tag: `git tag -l 'release/*' --sort=-v:refname | head -1`
2. If the release contains only fixes: increment patch (`Z`)
3. If the release contains new features: increment minor (`Y`), reset patch to 0
4. If the user explicitly requests a major version bump: increment major (`X`), reset minor and patch to 0 -- **never do this without explicit approval**

### Hotfixes

Hotfixes are committed directly on `master` in critical situations only. **ALWAYS requires explicit user approval. Every step is mandatory -- including PR and GitHub Release.**

**Step 1: Fix on master**

```bash
git checkout master
git pull origin master
# Make the fix
git add <files>
git commit -m 'fix: critical description of hotfix'
```

**Step 2: Create hotfix PR (MANDATORY)**

Even for hotfixes, a PR into master is required. Title: `release: vX.Y.Z`.

```bash
git push origin master  # Push the fix (with user approval)
# Create PR documenting the hotfix
gh pr create --base master --assignee @me --label bug --title 'release: vX.Y.Z' --body "$(cat <<'EOF'
## Summary

<What was fixed and why it was critical>
EOF
)"
```

**Step 3: Tag and push**

```bash
git tag release/v<version>
git push origin master --tags
```

**Step 4: Create GitHub Release (MANDATORY)**

```bash
gh release create release/v<version> --title 'vX.Y.Z' --notes "$(cat <<'EOF'
## Summary

<Same content as the hotfix PR body>
EOF
)"
```

**Step 5: Fast-forward develop**

```bash
git checkout develop
git pull origin develop
git merge --ff-only master
git push origin develop
```

**CRITICAL:** Same rule as releases -- NEVER merge master into develop. Only `--ff-only`. If it fails, the branches have diverged and you need to consult the user with options before proceeding.

## Claude Web Workaround

Claude Code on the web can only push to branches matching `claude/*<session-id>`. When changes need to land on `develop` or `master`, use this pattern:

### Push to temp branch

```bash
git push -u origin develop:claude/temp-develop-<session-id>
```

### User applies locally

```bash
git fetch origin claude/temp-develop-<session-id>
git checkout develop
git merge --ff-only origin/claude/temp-develop-<session-id>
git push origin develop
```

### Clean up

```bash
git push origin --delete claude/temp-develop-<session-id>
```

**Note:** This workaround is specific to the web environment. Claude Code running locally (terminal) can push directly to branches.

## Pull Requests

**CRITICAL: Always verify you are operating on the correct repository before running any `gh` command.** Run `gh repo view --json nameWithOwner -q '.nameWithOwner'` first to confirm.

### PR Writing Principles

- Analyze the **final diff** against the base branch (`git diff develop...HEAD`), not individual commit history
- Keep descriptions **high-level** -- do not reference files or code unless the change is crucial
- No filler words, no excessive adjectives, no overstructuring
- **ASCII only** -- no special characters, emojis, or bullet symbols like "•" (use markdown `-` instead)
- Do not duplicate the title in the description body
- Never include session-specific details (task numbers, agent names, internal debugging context)
- **Never list commits** in PR or release descriptions -- commit history is already visible in GitHub
- **No local/internal context** -- never reference local-only directories, developer-specific files, untracked files, or internal debugging artifacts. PR descriptions are public-facing; only include information meaningful to any reader. Bad: "pre-existing errors in untracked Traces/ directory". Good: "1 pre-existing failure (unrelated)".
- **Use proper punctuation** -- use periods and commas for natural sentence flow, not double dashes (`--`). Double dashes render poorly on GitHub and look unprofessional. Bad: "Pure refactor -- no behavior change." Good: "Pure refactor. No behavior change."
- **No self-confirmation.** Do not state that you followed rules or conventions (e.g., "updated both .claude/ and .cursor/", "used --no-ff merge"). The diff shows what was done. PR descriptions explain what changed and why, not that you did your job correctly.
- **Read before editing.** Before updating an existing PR description with `gh pr edit`, always read the current body first: `gh pr view <N> --json body -q '.body'`. Never overwrite blindly.
- **Finalize descriptions once, not incrementally.** Do not update PR descriptions while work is still in progress. Wait until all verification (tests, evaluations, e2e) is complete, then write the final description once. Incremental updates waste time and leave unfinished content visible in the PR.
- **No test plan section for documentation PRs.** Do not include a "Test plan" section in PRs for documentation, skills, or config-only changes. There is nothing executable to test. Only include a test plan when the PR contains code or behavior changes that require verification steps.

### Before Merging Any PR

**Always verify that the **Type Check** CI workflow has passed before merging.** This applies to all PRs -- feature branches into develop, release PRs into master, hotfixes, everything.

**To wait for checks to complete, use `--watch` -- never use `sleep`:**

```bash
# Watch until all checks finish (blocks until complete)
gh pr checks --watch

# Or watch a specific run by ID
gh run watch <run-id>
```

`gh pr checks` without `--watch` only shows the current snapshot. Use `--watch` so the process blocks until checks pass or fail, instead of polling with `sleep`.

**Do NOT merge if Type Check is failing.** Report the failure to the user and wait for their decision.

**Post-merge checklist (MANDATORY):**
- [ ] Delete local branch: `git branch -d <branch>`
- [ ] Delete remote branch: `git push origin --delete <branch>`
- [ ] Prune stale tracking refs: `git fetch origin --prune`

Do not consider the merge complete until all three steps are done.

### Self-Assignment and Labels

**Always self-assign and label PRs.** This is a required step, not optional. Can be done at creation time or immediately after. When labels and assignee are set at creation time (via `--label` and `--assignee @me` flags on `gh pr create`), no separate `gh pr edit` step is needed.

**Always discover available labels first:**
```bash
gh label list
```

Use the labels that actually exist on the repository. Different repos may have different label sets. Match the PR's commit-style prefix to the closest available label:

| Prefix | Look for labels matching |
|--------|-------------------------|
| `feat:` | enhancement, feature |
| `fix:` | bug |
| `docs:` | documentation |
| `refactor:` | refactoring |
| `test:` | testing |
| `release:` | release |
| `chore:` (infra/CI) | infrastructure, CI/CD |

Multiple labels can be applied when relevant (e.g., a fix that also touches infrastructure):
```bash
# At creation time (preferred) -- multiple --label flags for multiple labels
gh pr create --base develop --assignee @me --label 'bug' --label 'infrastructure' --title '...' --body '...'

# Or after creation
gh pr edit <number> --add-assignee @me --add-label 'documentation'
```

### Feature/Fix PRs (branch -> develop)

**Title:** Use commit-style prefixes (`feat:`, `fix:`, `refactor:`, etc.) consistently, same as commit messages.

**Body:** Start with a **Summary** of what changed and why. Then add whatever sections are relevant -- there is no fixed template. Structure it the same way as release descriptions: summary first, then dynamic sections that add value.

```bash
# Verify correct repo first
gh repo view --json nameWithOwner -q '.nameWithOwner'

# Create PR targeting develop
gh pr create --base develop --assignee @me --label <label> --title '<type>: <concise title>' --body "$(cat <<'EOF'
## Summary

<What changed and why>

<... additional sections as relevant, no fixed structure ...>
EOF
)"
```

### Release PRs (develop -> master)

**Title:** `release: vX.Y.Z`

**Body:** Start with a human-readable title (a descriptive name for the release). Then a **Summary** section that is always present. Beyond that, add whatever sections are relevant to this release -- there is no fixed template. Examples of sections that might appear: Highlights, Architecture Changes, Test Coverage, Infrastructure, Breaking Changes, etc.

```bash
gh pr create --base master --assignee @me --title 'release: vX.Y.Z' --body "$(cat <<'EOF'
# Human-Readable Release Title

## Summary

<What this release contains and why it matters>

<... additional sections as relevant, no fixed structure ...>
EOF
)"
```

### GitHub Releases

After tagging master (see Releases section), create a GitHub Release. The content should match the release PR body.

**Title:** `vX.Y.Z`

```bash
gh release create release/vX.Y.Z --title 'vX.Y.Z' --notes "$(cat <<'EOF'
# Human-Readable Release Title

## Summary

<Same content as the release PR body>

<... dynamic sections ...>
EOF
)"
```

**The full release flow in order:**
1. Prepare and create release PR (see Releases section steps 1-2)
2. Verify CI passes, merge locally, get user approval (steps 3-4)
3. Push master with tag (step 5)
4. Fast-forward develop (step 6)
5. Create GitHub Release (step 7)

## Multirepo Submodule Sync

The `isic-project` repository is a multirepo that references nested repos (backend, frontend, hardware) as git submodules. The submodule pointers must always reflect consistent, fully working states.

**Rule:** After completing a feature, fix, or any meaningful work in a nested repo (e.g., `backend`), update the multirepo's submodule pointer to that commit via a **separate PR**. This ensures the multirepo always points to commits where all subprojects are in a working state.

**Workflow:**

1. Complete work in the nested repo (merge PR, push to develop/main)
2. In the multirepo (`isic-project`):
   ```bash
   git checkout main
   git pull origin main
   git checkout -b chore/sync-submodules
   cd <submodule-dir>     # e.g., backend/
   git fetch origin
   git checkout <commit>  # the latest working commit
   cd ..
   git add <submodule-dir>
   git commit -m "chore: sync <submodule> to latest working state"
   git push -u origin chore/sync-submodules
   ```
3. Create a PR targeting `main` with assignee and label
4. Merge with `--no-ff`, clean up branch

**When to sync:**
- After merging any PR in a nested repo
- After a release in a nested repo
- Whenever the multirepo's submodule pointers are stale

**Do not** bundle submodule syncs with other changes. They are always a separate commit and PR.

## Forbidden Operations

| Operation | Why |
|-----------|-----|
| `git rebase master` | Rewrites shared production history |
| `git rebase develop` | Rewrites shared integration history |
| `git push --force <any-branch>` | Use `--force-with-lease` instead -- provides concurrent push protection |
| `git push --force origin master` | Destroys production history |
| `git push --force origin develop` | Destroys integration history |
| `git push origin master` (without approval) | Production auto-deploys |
| `git merge` without `--no-ff` | Loses branch history, no merge commit to track |
| Committing directly on `develop` | All work must go through feature/fix branches |
| `git merge master` into `develop` (non-ff) | Creates unnecessary merge commits; use `--ff-only` after releases |
| Commits that break the build | Every commit must be a working snapshot |
| Bundling unrelated changes in one commit | Separate logical changes into separate commits |
| AI attribution in commit messages | No "made with Claude", "Co-Authored-By", etc. |
| `git add -A` or `git add .` blindly | May stage sensitive files or unrelated changes |
| Skipping branch cleanup after merge | Leaves stale branches on local and remote; cleanup is mandatory, not optional |
| Bare `git stash` | Stashes ALL tracked changes, not just yours -- use `git stash push -- <files>` |
| `git stash -u` / `--include-untracked` | Captures untracked files, can destroy user's WIP files |
| `git stash drop` / `git stash clear` without checking | May destroy user's stashed work -- always `git stash show -p` first |

## Troubleshooting

### Branch Not Found Locally

When a requested branch cannot be found with `git branch -a`, always run `git fetch --all` before concluding it does not exist or asking the user. The branch may exist on the remote but not yet fetched locally (e.g., pushed from another device).

```bash
git fetch --all
git branch -a | grep <branch-name>
```

Only after fetching and still not finding it should you conclude the branch does not exist.

### Accidentally Committed to Wrong Branch

```bash
# Save the commit hash
git log --oneline -1

# Reset the wrong branch (if not pushed)
git reset --soft HEAD~1

# Switch to correct branch and apply
git checkout correct-branch
git commit -m "original message"
```

### Merge Conflict During --no-ff Merge

```bash
# After conflict during merge
git status  # See conflicted files
# Resolve conflicts in each file
git add <resolved-files>
git merge --continue
```

**Do NOT use `git merge --abort` without investigating** -- the conflict may contain important changes from both sides.

### Forgot --no-ff Flag

If you merged with fast-forward by accident (no merge commit created):

```bash
# If not pushed yet, reset and redo
git reset --hard HEAD~<number-of-commits-from-branch>
git merge --no-ff <branch>
```

If already pushed to `master`/`develop`, do NOT force push. The commits are on the target branch without a merge commit -- accept it and be more careful next time.

### Need to Verify a Partial Commit Works

When using `git add -p` or staging specific files for a focused commit:

```bash
# Stage your changes
git add -p <file>

# Stash ONLY tracked unstaged changes (see Stash Safety section)
# This does NOT touch untracked files
git stash push --keep-index

# Verify the staged-only state works
uv run ruff check .
uv run mypy .

# If good, commit
git commit -m 'fix: targeted change'

# Restore remaining work IMMEDIATELY
git stash pop
```

**WARNING:** Always pop the stash immediately after verification. See the Stash Safety section for full rules.
