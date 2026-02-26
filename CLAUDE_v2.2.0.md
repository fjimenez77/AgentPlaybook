# CLAUDE.md — Universal Development Agent Configuration

> **Version:** 2.2.0
> **Purpose:** This file configures the AI development agent's behavior for ANY project —
> from a 50-line Python script to a full-stack SaaS platform. It defines how the agent plans,
> builds, communicates, tracks work, maintains documentation, manages quality, preserves
> context across sessions, and integrates with version control.
>
> **Setup:** Place this file at your project root. On first run, use `//onboard` for existing
> projects or `//scaffold` for new ones. The agent adapts its behavior to your project scale.

---

## 🔧 PROJECT IDENTITY (configure per project)

> **Edit this section for each project. Everything else is universal.**

```yaml
PROJECT_NAME: "YourProjectName"
DESCRIPTION: "Brief description of what this project does"
INDUSTRY: "e.g., fintech, healthcare, e-commerce, SaaS"
MULTI_TENANT: true  # Does this serve multiple organizations/clients?

# ──────────────────────────────────────────────────
# PROJECT SCALE — Controls how much process the agent applies
# ──────────────────────────────────────────────────
# 🔴 FULL     — Full-stack apps, SaaS platforms, multi-service systems
#                All 11 docs required. Full slash commands. Auto-backup every 10 prompts.
#                Git branching with feature branches, PRs, protected main.
#
# 🟡 STANDARD — Medium apps, APIs, dashboards, multi-file projects
#                Core 6 docs required (CONTEXT, ROADMAP, BUGS, ARCHITECTURE, API_REFERENCE, CHANGELOG).
#                Others created on demand. Auto-backup every 15 prompts.
#                Git with feature branches, direct merge OK.
#
# 🟢 LITE     — Scripts, CLIs, single-page apps, prototypes, automation tools
#                Only 2 docs required (CONTEXT, CHANGELOG).
#                Others created only if needed. Auto-backup every 25 prompts.
#                Git with simple main-branch workflow.
#                Planning is lighter (no formal PLAN block for <3 file changes).
#                Architecture rules relaxed — functional/procedural OK if appropriate.
# ──────────────────────────────────────────────────
PROJECT_SCALE: "FULL"  # Options: FULL / STANDARD / LITE

TECH_STACK:
  backend: "e.g., FastAPI / Django / Express / Spring Boot"
  frontend: "e.g., React + TypeScript / Next.js / Vue / Angular"
  database: "e.g., PostgreSQL / MySQL / MongoDB"
  orm: "e.g., SQLAlchemy / Prisma / TypeORM / Hibernate"
  migrations: "e.g., Alembic / Prisma Migrate / Flyway"
  auth: "e.g., JWT / OAuth2 / Auth0 / Firebase Auth"
  realtime: "e.g., WebSocket / SSE / Socket.io"
  http_client: "e.g., httpx / axios / fetch"
  deployment: "e.g., Docker / K8s / Vercel / AWS"
  ci_cd: "e.g., GitHub Actions / GitLab CI / Jenkins"

# ──────────────────────────────────────────────────
# VERSION CONTROL — Git & GitHub Configuration
# ──────────────────────────────────────────────────
GIT:
  enabled: true
  remote: "github"                    # github / gitlab / bitbucket / none
  repo_url: ""                        # e.g., https://github.com/user/project
  default_branch: "main"              # main / master / develop
  branching_strategy: "feature"       # feature / trunk / gitflow (see Git section below)
  auto_commit: false                  # If true, agent commits after each completed task
  commit_convention: "conventional"   # conventional / simple (see Git section below)

  # ── Remote Platform Integration ──
  # These control what //get-bugs and //check-prs pull from the remote platform.
  # Set to false if your repo doesn't use a feature, or if you want to skip it.
  platform_features:
    issues: true                      # Pull open issues (filtered by 'bug' label)
    dependabot_alerts: true           # Pull Dependabot security vulnerability alerts
    dependabot_prs: true              # Pull pending Dependabot PRs (security + version updates)
    security_advisories: true         # Pull GitHub Security Advisories
    ci_status: true                   # Check CI/CD pipeline status (failing workflows)
    pull_requests: true               # Check open PRs for conflicts, failing checks, staleness

LANGUAGE_RULES:
  - "e.g., Python: from __future__ import annotations at top of every file"
  - "e.g., TypeScript: strict mode, no `any` types"
  - "e.g., Always use async/await for IO operations"

NAMING_CONVENTIONS:
  backend_files: "snake_case"
  frontend_files: "PascalCase"
  database_tables: "snake_case_plural"
  api_paths: "kebab-case"
  env_vars: "SCREAMING_SNAKE_CASE"
```

---

## 🔀 GIT & VERSION CONTROL

> The agent works locally first, then coordinates with the remote repository.
> This ensures code is always backed up, versioned, and recoverable.

### Branching Strategy (by project scale)

**FULL — Feature Branch Workflow:**
```
main (protected — always deployable)
  └── develop (integration branch — optional)
       ├── feature/T-201-lead-assignment
       ├── feature/T-202-email-templates
       ├── fix/BUG-003-token-expiry
       └── hotfix/critical-auth-bypass
```
- Every task gets its own branch: `feature/T-{id}-{short-name}` or `fix/BUG-{id}-{short-name}`
- Merge to main via PR/merge request (or direct if solo developer)
- Tag releases: `v1.0.0`, `v1.1.0`, etc.
- Never commit directly to main unless it's a 1-line hotfix

**STANDARD — Simplified Feature Branches:**
```
main
  ├── feature/add-user-dashboard
  ├── fix/login-bug
  └── (merge directly to main when ready)
```
- Feature branches for multi-file changes
- Direct commits to main OK for single-file fixes
- Tag milestones: `v0.1.0`, `v0.2.0`, etc.

**LITE — Trunk-Based:**
```
main (all commits go here)
```
- Commit directly to main
- Tag versions only for significant releases
- Keep it simple — the project is small

### Commit Message Convention

**Conventional Commits (default):**
```
type(scope): short description

Types:
  feat     — New feature                    feat(leads): add bulk assignment endpoint
  fix      — Bug fix                        fix(auth): resolve token refresh race condition
  refactor — Code restructure, no behavior change   refactor(services): extract email adapter
  docs     — Documentation only             docs: update API_REFERENCE with new endpoints
  test     — Add or fix tests               test(leads): add assignment integration tests
  chore    — Build, config, tooling          chore: update dependencies
  migrate  — Database migration              migrate: add assigned_to column to leads
  style    — Formatting, no logic change     style: fix linting warnings
  perf     — Performance improvement         perf(queries): add index on leads.status
  ci       — CI/CD changes                   ci: add staging deploy workflow

Body (optional): Explain WHY, not what (the diff shows what).
Footer: Reference task/bug IDs — "Closes T-201" or "Fixes BUG-003"
```

**Simple Commits (for LITE projects):**
```
[verb] what changed — e.g., "Add CSV export script" / "Fix date parsing bug"
```

### Git Workflow Integration with Agent

**When the agent completes a task:**
1. Present the DONE summary as usual
2. If `GIT.auto_commit` is true → commit with conventional message automatically
3. If `GIT.auto_commit` is false → suggest a commit message:
   ```
   Suggested commit:
     git add -A
     git commit -m "feat(leads): add bulk assignment endpoint

   Closes T-201"
     git push origin feature/T-201-lead-assignment
   ```
4. User decides when to commit/push

**When the agent starts a new feature:**
1. Suggest creating a feature branch (FULL/STANDARD scale)
2. Name the branch based on task ID: `feature/T-{id}-{short-name}`

**Backup & Recovery:**
- `//git-backup` — Stage all changes, commit with a work-in-progress message, push to remote
- `//git-status` — Show current branch, uncommitted changes, commits ahead/behind remote
- If the agent detects uncommitted changes at session start, it flags them:
  ```
  ⚠️ Uncommitted changes detected: [N files modified, M files new]
  Recommend: commit or stash before proceeding.
  ```

### .gitignore Requirements

The agent ensures these are NEVER committed:
```
# Environment & secrets
.env
.env.local
.env.production
*.pem
*.key

# Dependencies
node_modules/
__pycache__/
*.pyc
.venv/
venv/

# IDE
.vscode/
.idea/
*.swp

# OS
.DS_Store
Thumbs.db

# Build artifacts
dist/
build/
*.egg-info/
```

---

## 🔄 EXISTING PROJECT ONBOARDING (`//onboard`)

> **For when you drop CLAUDE.md into an existing project.**
> The agent does NOT assume the project is broken or needs rewriting.
> It analyzes, documents, and gently brings the project up to standard.

### Onboarding Protocol

When `//onboard` is called, the agent executes this sequence:

**Phase 1 — Discovery (read-only, no changes)**
```
ONBOARDING: Phase 1 — Discovery

Scanning project structure...

1. 📂 File structure analysis
   - Languages detected: [Python, TypeScript, etc.]
   - Framework detected: [FastAPI, React, etc.]
   - Total files: [N] | Total lines: [~N]

2. 📊 Architecture assessment
   - Backend structure: [monolithic / layered / feature-based / microservices]
   - Frontend structure: [SPA / MPA / SSR / static]
   - Database: [detected or "none found"]
   - Existing tests: [Y/N, framework, count]

3. 🔀 Git status
   - Git initialized: [Y/N]
   - Remote: [URL or "none"]
   - Branches: [list]
   - Last commit: [date + message]
   - Uncommitted changes: [Y/N]

4. 📄 Existing documentation
   - README: [Y/N — quality: good/basic/minimal/none]
   - API docs: [Y/N]
   - Architecture docs: [Y/N]
   - Other docs found: [list]

5. 🔍 Codebase health indicators
   - Estimated endpoint count: [N]
   - Estimated model/entity count: [N]
   - Estimated page/view count: [N]
   - Hardcoded secrets detected: [Y/N — flag immediately]
   - Test coverage: [estimated or "unknown"]
```

**Phase 2 — Compliance Gap Analysis**
```
ONBOARDING: Phase 2 — Gap Analysis

Based on PROJECT_SCALE: [FULL/STANDARD/LITE], here's what's needed:

Required docs:
  ✅ Already exists: [list]
  ⬜ Missing (will create): [list]
  ⚠️ Exists but incomplete: [list]

Git compliance:
  ✅ / ⬜ Git initialized
  ✅ / ⬜ Remote configured
  ✅ / ⬜ .gitignore present and adequate
  ✅ / ⬜ No secrets in committed files

Code compliance:
  ✅ / ⬜ Code follows layered architecture
  ⚠️ [Areas that could use improvement — NOT blockers]
  📝 [Recommendations for future refactoring — added to Tech Debt Register]

Estimated onboarding effort: [Small / Medium / Large]
```

**Phase 3 — Scaffold Missing Docs (with approval)**
```
ONBOARDING: Phase 3 — Setup

I'll now create the missing documentation by analyzing the existing codebase:

  1. docs/CONTEXT.md — Populated with discovered project state
  2. docs/ARCHITECTURE.md — Generated from actual code structure
  3. docs/API_REFERENCE.md — Generated from actual endpoint scan
  4. docs/ROADMAP.md — Initialized with any TODO/FIXME items found
  5. docs/BUGS.md — Initialized (empty or with known issues)
  6. [etc. based on what's missing]

I will NOT modify any existing code or project files.
I will ONLY add docs/ files.

Shall I proceed?
```

**Phase 4 — Ready**
```
✅ ONBOARDING COMPLETE — [PROJECT_NAME]

Project scale: [FULL/STANDARD/LITE]
Documents created: [N]
Documents updated: [N]
Git status: [configured / needs setup]

Tech debt items logged: [N] (see docs/RISKS.md)
Recommended first actions:
  1. [Most important action]
  2. [Second priority]
  3. [Third priority]

Run //catch-up anytime to get a full status briefing.
Run //health-check to see project health at a glance.
```

### Onboarding Principles

- **NEVER rewrite or restructure existing code** during onboarding
- **NEVER break anything** — onboarding is documentation-only
- **Respect existing patterns** — document them, don't judge them
- **Log improvements as tech debt** — don't force them
- **Existing README.md is preserved** — docs/ is additive
- **If the project has its own conventions**, document them in CONTEXT.md
- **If the project uses a different folder structure** than the recommended one, adapt to IT, don't fight it

---

## 📐 PROJECT SCALE — Adaptive Behavior

The agent adjusts its process overhead based on `PROJECT_SCALE`:

### Behavior Matrix

| Behavior                          | 🔴 FULL           | 🟡 STANDARD        | 🟢 LITE             |
|-----------------------------------|--------------------|---------------------|----------------------|
| **Required docs**                 | All 11             | Core 6              | 2 (CONTEXT+CHANGELOG)|
| **Plan before building**          | Always (PLAN block)| Always (can be brief)| Only if 3+ files     |
| **Auto-backup interval**          | Every 10 prompts   | Every 15 prompts    | Every 25 prompts     |
| **Full doc sync interval**        | Every 30 prompts   | Every 45 prompts    | On demand only       |
| **Git branching**                 | Feature branches   | Feature branches OK  | Trunk (main only)    |
| **Commit messages**               | Conventional       | Conventional         | Simple               |
| **ARCHITECTURE.md**               | Required           | Required             | Optional             |
| **API_REFERENCE.md**              | Required           | Required             | Only if has API       |
| **DATA_MODELS.md**                | Required           | On demand            | Optional             |
| **DECISIONS.md**                  | Required           | On demand            | Optional             |
| **INTEGRATIONS.md**               | Required           | On demand            | Optional             |
| **QA.md**                         | Required           | On demand            | Optional             |
| **BUGS.md**                       | Required           | Required             | Optional             |
| **RISKS.md**                      | Required           | On demand            | Optional             |
| **OOP/Layered architecture**      | Strict             | Encouraged           | If appropriate        |
| **Functional/procedural style**   | Only in utilities   | Acceptable           | Perfectly fine        |
| **Error handling rigor**          | Full (try/catch everywhere) | Standard    | Basic (print/exit OK) |
| **Session start briefing**        | Full status        | Brief status         | Just "Ready"         |

### When to Use Each Scale

**🔴 FULL** — Use for:
- Production SaaS applications
- Multi-service / microservice architectures
- Team projects with multiple developers
- Projects with external API consumers
- Anything with a database, auth system, and frontend

**🟡 STANDARD** — Use for:
- Internal tools and dashboards
- APIs with 10-50 endpoints
- Medium-complexity single-developer projects
- Projects that might grow to FULL later

**🟢 LITE** — Use for:
- Python scripts and CLIs
- Single HTML/CSS/JS pages
- Automation scripts and cron jobs
- Data processing pipelines
- Quick prototypes and experiments
- Browser extensions
- Configuration tools

**Upgrading scale:** If a LITE project grows, just change the setting to STANDARD or FULL
and run `//onboard` — the agent will scaffold the additional docs without disrupting anything.

---

## 📁 DOCUMENTATION FRAMEWORK

The `docs/` directory is the project's living knowledge base. **The agent is responsible for
keeping ALL documents current.** When any feature is added, modified, or removed, ALL
relevant documents must be updated in the same work session.

### Required Documents (by project scale)

```
docs/
├── CONTEXT.md             ← 🔴🟡🟢 Session memory — rolling context for continuity
├── CHANGELOG.md           ← 🔴🟡🟢 Version history, what changed and when
├── ROADMAP.md             ← 🔴🟡   Feature tracker, phases, task status, build sequence
├── BUGS.md                ← 🔴🟡   Bug tracker with severity, reproduction, status
├── ARCHITECTURE.md        ← 🔴🟡   System design, models, services, data flow diagrams
├── API_REFERENCE.md       ← 🔴🟡   Full API catalog — every endpoint, schema, auth, examples
├── RISKS.md               ← 🔴     Blockers, risks, dependencies, mitigation plans
├── QA.md                  ← 🔴     Test plans, test cases, coverage gaps, QA status
├── DATA_MODELS.md         ← 🔴     Entity relationship definitions, field types, constraints
├── DECISIONS.md           ← 🔴     Architecture Decision Records (ADRs)
├── INTEGRATIONS.md        ← 🔴     External service connections, credentials, status, limits
└── codex/                 ← 🔴🟡   Feature specification documents (detailed build prompts)
    └── [FeatureName]_Spec.md

🔴 = Required for FULL | 🟡 = Required for STANDARD | 🟢 = Required for LITE
Documents not required for your scale are created on-demand when needed.
```

### Document Update Rules

**CRITICAL: Documents are never "done." They evolve with the codebase.**

| When this happens...                  | Update these documents...                                                     |
|---------------------------------------|-------------------------------------------------------------------------------|
| New feature built                     | ROADMAP, ARCHITECTURE, API_REFERENCE, DATA_MODELS, QA, CHANGELOG             |
| Bug found                             | BUGS (add entry), QA (add regression test)                                    |
| Bug fixed                             | BUGS (mark resolved), CHANGELOG, QA (verify test added)                       |
| Architecture decision made            | DECISIONS, ARCHITECTURE                                                       |
| New risk or blocker identified        | RISKS                                                                         |
| Blocker resolved                      | RISKS (move to resolved), ROADMAP (unblock tasks)                             |
| Database model changed                | DATA_MODELS, ARCHITECTURE, API_REFERENCE (if endpoints affected)              |
| Session ending or context compacting  | CONTEXT (update with latest state)                                            |
| New API endpoint added                | API_REFERENCE, ARCHITECTURE, QA (add endpoint tests)                          |
| API endpoint modified or removed      | API_REFERENCE (update/deprecate), CHANGELOG, QA                               |
| External integration added/changed    | INTEGRATIONS, ARCHITECTURE, RISKS (evaluate dependency risk)                  |
| Dependency added/changed              | ARCHITECTURE, RISKS (evaluate dependency risk)                                |
| Phase or milestone completed          | ROADMAP, CHANGELOG, CONTEXT                                                   |
| Every 10 prompts (auto)              | CONTEXT (periodic backup — see Auto-Backup System)                            |
| Remote scan (`//get-bugs`)           | BUGS (new entries), RISKS (security/CVEs), CONTEXT (scan log), ROADMAP (if blockers found) |

---

## ⚡ SLASH COMMANDS — Quick Action System

The agent recognizes `//command` patterns as immediate action triggers.
**These execute immediately — no confirmation needed unless the command says otherwise.**

### Context & Memory Commands

| Command                   | Action                                                                                          |
|---------------------------|-------------------------------------------------------------------------------------------------|
| `//context-backup`        | Immediately update `docs/CONTEXT.md` with full current state: decisions made, work done, in-progress items, next steps, user preferences learned. Confirm when done. |
| `//catch-up`              | **New session onboarding.** Read ALL docs/ files (CONTEXT → ROADMAP → RISKS → BUGS → ARCHITECTURE → API_REFERENCE → DECISIONS), then present a full status briefing. This is the "get up to speed" command. |
| `//where-are-we`          | Quick status — read CONTEXT.md + ROADMAP.md, present: current phase, last task, next priority, blockers count, bugs count. |
| `//handoff`               | Full platform handoff protocol. Update ALL docs to current state, ensure CONTEXT.md is comprehensive, present a summary. After this, any agent with the project folder can resume. |

### Planning & Roadmap Commands

| Command                   | Action                                                                                          |
|---------------------------|-------------------------------------------------------------------------------------------------|
| `//update-roadmap`        | Read current `docs/ROADMAP.md`, sync it with actual project state: mark completed tasks ✅, update in-progress items, recalculate phase percentages, update build metrics table. Confirm changes. |
| `//new-feature [name]`    | **Full new feature analysis pipeline:** 1) Read ARCHITECTURE.md + ROADMAP.md + RISKS.md + API_REFERENCE.md + DATA_MODELS.md 2) Analyze how the new feature fits with existing architecture 3) Identify which existing systems it touches or depends on 4) Identify blockers, risks, and prerequisites 5) Propose where it fits in the roadmap (which phase, priority) 6) List ALL documents that would need updating 7) Present a build plan with scope estimate (S/M/L/XL) 8) Wait for approval before any code changes. |
| `//priorities`            | Read ROADMAP + RISKS + BUGS, recommend the top 3 highest-impact unblocked tasks with reasoning. |

### Documentation Commands

| Command                   | Action                                                                                          |
|---------------------------|-------------------------------------------------------------------------------------------------|
| `//update-documents`      | **Full documentation sync.** Read the codebase and all docs/ files, identify ANY docs that are out of date or incomplete, update them all. Report what was changed. This is the "make sure everything is current" command. |
| `//update-api`            | Scan all route/controller files, compare against `docs/API_REFERENCE.md`, add any missing endpoints, update changed ones, flag any that no longer exist. |
| `//update-models`         | Scan all model/entity files, compare against `docs/DATA_MODELS.md`, sync any differences. |
| `//update-architecture`   | Review current codebase structure and update `docs/ARCHITECTURE.md` to reflect actual state. |

### Bug & QA Commands

| Command                   | Action                                                                                          |
|---------------------------|-------------------------------------------------------------------------------------------------|
| `//log-bug [description]` | Create a new entry in `docs/BUGS.md` with next available BUG-ID, fill in as much as can be determined (severity, component, related files). Ask for any missing reproduction steps. |
| `//bugs`                  | Read `docs/BUGS.md`, present: open bugs by severity, oldest unresolved, recommended fix priority. |
| `//qa-status`             | Read `docs/QA.md`, present: coverage gaps, pending tests, what needs testing most urgently. |

### Build & Fix Commands

| Command                   | Action                                                                                          |
|---------------------------|-------------------------------------------------------------------------------------------------|
| `//fix [description]`     | Bug fix pipeline: reproduce → root cause → plan → **wait for approval** → fix → update BUGS.md + QA.md + CHANGELOG.md. |
| `//build [feature]`       | Build pipeline: check codex/ for spec → check ROADMAP for fit → check RISKS for blockers → plan → **wait for approval** → build → update all relevant docs. |
| `//refactor [target]`     | Refactor pipeline: analyze current code → identify issues → propose refactor plan with before/after architecture → **wait for approval** → execute → update docs. |

### Project Setup Commands

| Command                   | Action                                                                                          |
|---------------------------|-------------------------------------------------------------------------------------------------|
| `//scaffold`              | Create the full `docs/` folder structure with populated template files (for NEW projects).       |
| `//onboard`               | **Existing project onboarding.** Analyze codebase, identify gaps, create missing docs without touching code. See Existing Project Onboarding section. |
| `//health-check`          | Run a project health diagnostic: check for missing docs, outdated docs, untracked bugs, missing tests, architectural concerns. Report findings. |
| `//dependency-audit`      | Review all external dependencies, check for known issues, outdated versions, security concerns. Update RISKS.md with findings. |

### Git & Version Control Commands

| Command                   | Action                                                                                          |
|---------------------------|-------------------------------------------------------------------------------------------------|
| `//git-backup`            | Stage all changes, commit with WIP message (`chore: work-in-progress backup [timestamp]`), push to remote. Safety net — not a clean commit. |
| `//git-status`            | Show: current branch, uncommitted changes, commits ahead/behind remote, last commit info.       |
| `//git-commit [message]`  | Stage all changes, commit with provided message (or suggest one based on work done). Does NOT push. |
| `//git-push`              | Push current branch to remote. If on main with uncommitted feature work, warn first.            |
| `//git-branch [name]`     | Create and switch to a new feature branch. Auto-names based on current task if no name given.   |
| `//git-merge`             | Merge current feature branch into default branch. Warns about uncommitted changes first.        |
| `//git-tag [version]`     | Create a git tag for release versioning. Suggests next version based on CHANGELOG.md.           |
| `//get-bugs`              | **Remote platform scan.** Pull bugs, alerts, and issues from GitHub/GitLab/Bitbucket — Dependabot alerts, open issues, security advisories, CI failures. Analyze, classify, cross-reference with existing docs, present action plan, update BUGS.md + RISKS.md + CONTEXT.md. See Remote Platform Scanning section. |
| `//check-prs`             | **Pull request health check.** Scan open PRs for: failing checks, merge conflicts, stale PRs (no activity 7+ days), pending Dependabot PRs ready to merge. Present status + recommendations. |

---

## 🌍 REMOTE PLATFORM SCANNING (`//get-bugs` & `//check-prs`)

> Pulls bugs, alerts, vulnerabilities, issues, and PR status directly from the remote
> platform (GitHub, GitLab, Bitbucket). Ingests them into the project's documentation
> system so nothing falls through the cracks.
>
> **Requires:** `GIT.remote` set to a platform (not "none") and `GIT.platform_features`
> configured to indicate which features are active on the repo.

### `//get-bugs` — Full Remote Bug & Alert Ingestion

**Step 1 — Pull from the remote platform:**

Check each source that is enabled in `GIT.platform_features`:

| Source                     | What to pull                                                        | Platform setting            |
|----------------------------|---------------------------------------------------------------------|-----------------------------|
| **Issues**                 | Open issues labeled `bug`, `defect`, or `error` (configurable)      | `platform_features.issues`  |
| **Dependabot Alerts**      | Active vulnerability alerts with CVE IDs and CVSS severity scores   | `platform_features.dependabot_alerts` |
| **Dependabot PRs**         | Pending PRs from Dependabot (security patches + version updates)    | `platform_features.dependabot_prs` |
| **Security Advisories**    | Repository-level security advisories                                | `platform_features.security_advisories` |
| **CI/CD Status**           | Failing workflow runs on default branch                             | `platform_features.ci_status` |
| **Open PRs with failures** | PRs with failing checks or merge conflicts                          | `platform_features.pull_requests` |

**Step 2 — Analyze & Classify:**

For each finding:
- **Map severity** using Dependabot CVSS scores, issue labels, or CI criticality:
  - CVSS 9.0+ or CI failure on main → Critical
  - CVSS 7.0-8.9 or `priority: high` label → High
  - CVSS 4.0-6.9 or general `bug` label → Medium
  - CVSS < 4.0 or `cosmetic`/`minor` label → Low
- **Identify component** affected (backend / frontend / dependency / infra / CI)
- **Cross-reference** against existing `docs/BUGS.md` to detect duplicates:
  - Match by issue number, CVE ID, or title similarity
  - If already tracked → mark as "ALREADY TRACKED" with existing BUG-ID
  - If net-new → mark as "NEW" for ingestion

**Step 3 — Present consolidated report:**

```
REMOTE SCAN: [PROJECT_NAME] — [date]
Platform: GitHub | Repo: [repo_url]

🔴 Critical: [N]  |  🟠 High: [N]  |  🟡 Medium: [N]  |  🟢 Low: [N]

═══ NEW FINDINGS (not yet in BUGS.md) ═══

  1. 🔴 [Dependabot] lodash prototype pollution — Critical (CVSS 9.8)
     CVE-2025-XXXX | Affects: backend | Fix: bump lodash 4.17.19 → 4.17.21
     → Will add as BUG-[next] + RISKS.md security entry

  2. 🟠 [Issue #34] Login fails on Safari mobile — High
     Reported: 3 days ago | Labels: bug, frontend | Assigned: none
     → Will add as BUG-[next]

  3. 🟠 [CI] Backend test suite failing on main — High
     Last 2 runs failed | Workflow: test-backend.yml | Error: timeout
     → Will add as BUG-[next] (infra)

  4. 🟡 [Dependabot] axios ReDoS vulnerability — Medium (CVSS 5.3)
     CVE-2025-YYYY | Fix available: bump 0.21 → 1.7.2
     → Will add as BUG-[next] + RISKS.md

═══ ALREADY TRACKED ═══

  5. [Issue #28] = BUG-005 in BUGS.md — still open, no change
  6. [Issue #19] = BUG-002 in BUGS.md — resolved on platform, not yet closed in BUGS.md
     → Will update BUG-002 to RESOLVED

═══ DEPENDABOT PRs READY TO MERGE ═══

  7. PR #89: Bump axios 0.21 → 1.7.2 (security fix) — ready, checks passing
  8. PR #91: Bump postcss 8.2 → 8.4.31 (version update) — ready, checks passing
  9. PR #87: Bump webpack 5.75 → 5.90 (version update) — has merge conflict

═══ CI/CD STATUS ═══

  Pipeline: test-backend — ❌ FAILING (last 2 runs)
  Pipeline: test-frontend — ✅ PASSING
  Pipeline: deploy-staging — ⏸️ BLOCKED by failing backend tests

Recommended priority:
  1. Merge PR #89 (critical security fix — 2 min, no code change needed)
  2. Fix CI failure (blocks all merges to main)
  3. Address Issue #34 (user-facing Safari bug — High)
  4. Merge PR #91 (safe version bump)
  5. Resolve PR #87 merge conflict, then merge
```

**Step 4 — Update docs (with confirmation):**

```
I'll update the following docs:

  docs/BUGS.md:
    + BUG-010: lodash prototype pollution (Critical, Source: Dependabot CVE-2025-XXXX)
    + BUG-011: Login fails on Safari (High, Source: GitHub Issue #34)
    + BUG-012: Backend CI failing on main (High, Source: CI pipeline)
    + BUG-013: axios ReDoS vulnerability (Medium, Source: Dependabot CVE-2025-YYYY)
    ~ BUG-002: Updated to RESOLVED (closed on GitHub)

  docs/RISKS.md:
    + Security: CVE-2025-XXXX lodash (Critical, fix available)
    + Security: CVE-2025-YYYY axios (Medium, fix available)
    + Tech Debt: 2 Dependabot version PRs pending merge

  docs/CONTEXT.md:
    + Remote scan performed [date] — 4 new bugs ingested, 1 resolved

  docs/ROADMAP.md:
    + CI fix flagged as blocker for current phase (blocks merges)

Shall I proceed with these updates?
```

**Step 5 — Offer action plan:**

```
What would you like to do?
  A. Start fixing the highest-priority item (Critical Dependabot vuln)
  B. Merge the safe Dependabot PRs first (quick security wins)
  C. Fix the CI failure first (unblocks everything)
  D. Just update the docs and let me know what to tackle
```

### `//check-prs` — Pull Request Health Check

Focused specifically on open PR status (subset of `//get-bugs`):

```
PR HEALTH CHECK: [PROJECT_NAME] — [date]

Open PRs: [N total]

  ✅ HEALTHY (checks passing, no conflicts):
    PR #92: feat(leads): add bulk export — ready for review
    PR #93: fix(auth): token refresh — approved, ready to merge

  ❌ FAILING CHECKS:
    PR #85: feat(dashboard): analytics widget — 2 checks failing
      → test-frontend: TypeError in AnalyticsChart.test.tsx
      → lint: 3 warnings treated as errors

  ⚠️ MERGE CONFLICTS:
    PR #87: chore(deps): bump webpack — conflict in package-lock.json

  🕐 STALE (no activity 7+ days):
    PR #79: feat(reports): PDF export — last activity 12 days ago
      → May need rebase against main

  🤖 DEPENDABOT (ready to merge):
    PR #89: Bump axios 0.21→1.7.2 (security) — ✅ all checks pass
    PR #91: Bump postcss 8.2→8.4.31 (version) — ✅ all checks pass

Recommendations:
  1. Merge PR #93 (approved + passing)
  2. Merge Dependabot PRs #89, #91 (safe, checks passing)
  3. Fix failing checks on PR #85 before it goes stale
  4. Rebase PR #79 or close if abandoned
```

### Ingestion Rules

- **Never create duplicate entries** — always cross-reference by issue number, CVE ID, or PR number
- **Preserve source traceability** — every ingested bug includes `Source: GitHub Issue #XX` or `Source: Dependabot CVE-XXXX`
- **Severity comes from the platform** — use Dependabot CVSS scores, CI failure impact, and issue labels; don't invent severity
- **Resolved items sync both ways** — if a GitHub issue is closed but BUGS.md still shows open, update BUGS.md to RESOLVED
- **Dependabot PRs are not bugs** — they go in RISKS.md (Tech Debt / Security section) unless the vulnerability is actively exploitable (then also BUGS.md)
- **CI failures are infra bugs** — tracked in BUGS.md with component: `infra/ci`
- **All ingestion is logged in CONTEXT.md** with timestamp and counts

---

## ⏱️ AUTO-BACKUP SYSTEM — Periodic Context Preservation

> **Problem:** Context can be lost when conversations compact, sessions time out, or
> the user forgets to save. Relying solely on manual triggers or compaction detection is risky.
>
> **Solution:** The agent proactively saves context at regular intervals.

### Prompt Counter Rules

The agent maintains an **internal prompt counter** that tracks meaningful exchanges
(questions + responses that involve code, decisions, or progress — not casual chat).

| Trigger                                    | 🔴 FULL    | 🟡 STANDARD | 🟢 LITE    | Action                                    |
|--------------------------------------------|------------|-------------|------------|-------------------------------------------|
| **Periodic silent backup**                 | 10 prompts | 15 prompts  | 25 prompts | Auto-update `docs/CONTEXT.md` silently    |
| **Checkpoint with message**                | 20 prompts | 30 prompts  | 50 prompts | Update CONTEXT.md + brief checkpoint msg  |
| **Full doc sync**                          | 30 prompts | 45 prompts  | On demand  | CONTEXT + ROADMAP + CHANGELOG + dirty docs|
| **After any major milestone**              | Immediate  | Immediate   | Immediate  | Full backup regardless of counter         |
| **User says `//context-backup`**           | Immediate  | Immediate   | Immediate  | Full backup, resets counter               |
| **Conversation compaction detected**       | Emergency  | Emergency   | Emergency  | Backup ALL docs before context is lost    |

### What Gets Saved (Auto-Backup)

At minimum, every auto-backup updates CONTEXT.md with:
- Tasks completed since last backup
- Decisions made and reasoning
- Current state of in-progress work
- Any new blockers or bugs discovered
- User preferences or clarifications learned
- Files modified since last backup
- Next steps / priorities

### Checkpoint Message Format

```
📋 Checkpoint (prompt ~20): Completed [X, Y]. Currently working on [Z].
   Context saved to docs/CONTEXT.md.
   [1 critical item if any, e.g., "Note: BUG-003 still open — auth token expiry"]
```

Keep checkpoint messages brief — 2-3 lines max. Don't interrupt flow.

---

## 🧠 CONTEXT MEMORY SYSTEM (docs/CONTEXT.md)

> This is the most critical document for continuity. It serves as the agent's persistent
> memory across sessions, platforms, conversation compactions, and handoffs.

### Purpose

When conversations are compacted, sessions end, or the project moves to a different AI
platform or session, CONTEXT.md provides everything needed to resume work intelligently.
Any agent reading this file plus the other docs should be able to get fully up to speed
without any prior conversation history.

### Format

```markdown
# [PROJECT_NAME] — Development Context

> Last updated: [ISO timestamp]
> Current agent session: [session ID or platform identifier]
> Prompt counter: [N] since last full backup
> Auto-backup #: [N]

## Current State Summary
- **Active phase:** [Phase name from ROADMAP.md]
- **Current sprint/focus:** [What we're actively building]
- **Last completed task:** [Task name + brief result]
- **Next priority:** [What should be built next + why]
- **Active blockers:** [Count + brief list, or "None"]
- **Open bugs:** [Count by severity, or "None"]

## Recent Decisions & Context
<!-- Rolling log — keep last 20-30 entries, archive older ones -->

### [Date] — [Decision/Event Title]
- **What:** [What happened or was decided]
- **Why:** [Reasoning or trigger]
- **Impact:** [What this affects going forward]
- **Files touched:** [Key files modified]

### [Date] — [Previous entry]
...

## Conversation Continuity Notes
<!-- Things the agent should "remember" that aren't captured elsewhere -->
- [User preference or instruction that affects how work is done]
- [Clarification about business logic that came up in discussion]
- [Known quirk, workaround, or tech debt that needs awareness]
- [Pending user decisions that haven't been resolved yet]

## Architecture Snapshot
<!-- Quick reference so agent doesn't need to read full ARCHITECTURE.md every time -->
- **Key services:** [List of core modules/services]
- **Current model count:** [N models]
- **Current endpoint count:** [N endpoints]
- **Current page/view count:** [N pages]
- **Database migration head:** [Latest migration identifier]

## Onboarding Instructions
<!-- For a new agent session or platform handoff — triggered by //catch-up -->
1. Read this file (CONTEXT.md) for current state
2. Read ROADMAP.md for task status and priorities
3. Read RISKS.md for active blockers
4. Read BUGS.md for open issues
5. Read ARCHITECTURE.md for system design
6. Read API_REFERENCE.md for endpoint catalog
7. Read INTEGRATIONS.md for external service status
8. Check DECISIONS.md for any recent ADRs
9. Resume from "Next priority" above
```

### Update Triggers

**CONTEXT.md must be updated when:**
- A task is completed
- A meaningful decision is made during conversation
- The user shares a preference or clarification that should persist
- Before any session ends (if possible)
- At the beginning of a session (verify current state is accurate)
- When conversation compaction is detected or suspected
- On the auto-backup schedule (every 10/20/30 prompts)
- When `//context-backup` is called
- When `//handoff` is called

---

## 🌐 API REFERENCE (docs/API_REFERENCE.md)

> **The definitive catalog of every API endpoint in the project.**
> This document is the single source of truth for what the API does, how to call it,
> what it returns, and what permissions are required.

### Why This Exists

- New team members or AI agents need to understand the full API surface quickly
- Prevents duplicate endpoints from being built
- Ensures frontend and backend stay in sync
- Serves as living documentation for API consumers (internal and external)
- Catches orphaned endpoints (endpoints that no longer have a frontend consumer)

### Format

```markdown
# [PROJECT_NAME] — API Reference

> Last updated: [date]
> Total endpoints: [N]
> Base URL: [e.g., /api/v1]

## Quick Reference Table

| Method | Path                          | Description                    | Auth    | Permission         |
|--------|-------------------------------|--------------------------------|---------|---------------------|
| POST   | /api/auth/login               | Authenticate user              | None    | Public              |
| GET    | /api/leads                    | List leads (paginated)         | JWT     | leads:read          |
| POST   | /api/leads                    | Create a new lead              | JWT     | leads:create        |
| GET    | /api/leads/{id}               | Get lead details               | JWT     | leads:read          |
| PUT    | /api/leads/{id}               | Update lead                    | JWT     | leads:update        |
| DELETE | /api/leads/{id}               | Soft-delete lead               | JWT     | leads:delete        |
| ...    | ...                           | ...                            | ...     | ...                 |

## Endpoint Groups

### 🔐 Auth — `/api/auth/`

#### POST `/api/auth/login`
- **Description:** Authenticate user with email/password, return JWT tokens
- **Auth:** None (public)
- **Rate limit:** 5 attempts / minute per IP
- **Request body:**
  ```json
  {
    "email": "user@example.com",
    "password": "string"
  }
  ```
- **Success response (200):**
  ```json
  {
    "access_token": "eyJ...",
    "refresh_token": "eyJ...",
    "token_type": "bearer",
    "expires_in": 3600,
    "user": {
      "id": 1,
      "email": "user@example.com",
      "role": "admin"
    }
  }
  ```
- **Error responses:**
  - `401` — Invalid credentials
  - `429` — Rate limited
  - `422` — Validation error (missing fields)
- **Frontend consumer:** `LoginPage.tsx` → `api/auth.ts`
- **Backend file:** `backend/api/auth.py` → `login()`
- **Notes:** Returns both access (short-lived) and refresh (long-lived) tokens

#### POST `/api/auth/refresh`
...

### 👥 Leads — `/api/leads/`

#### GET `/api/leads`
- **Description:** List leads with pagination, filtering, and sorting
- **Auth:** JWT required
- **Permission:** `leads:read`
- **Query parameters:**
  | Param    | Type    | Default | Description              |
  |----------|---------|---------|--------------------------|
  | page     | int     | 1       | Page number              |
  | per_page | int     | 25      | Items per page (max 100) |
  | sort     | string  | -created_at | Sort field (- for desc)  |
  | status   | string  | null    | Filter by status         |
  | search   | string  | null    | Full-text search         |
- **Success response (200):**
  ```json
  {
    "items": [...],
    "total": 150,
    "page": 1,
    "per_page": 25,
    "pages": 6
  }
  ```
- **Scoping:** Results filtered by `user.current_org_id` (multi-tenant)
- **Frontend consumer:** `LeadsListPage.tsx` → `api/leads.ts`
- **Backend file:** `backend/api/leads.py` → `list_leads()`

### [Next group...]
...

## Deprecated Endpoints
<!-- Endpoints scheduled for removal — include removal date and replacement -->

| Method | Path                  | Deprecated Since | Removal Date | Replacement          |
|--------|-----------------------|------------------|--------------|----------------------|
| GET    | /api/v1/old-endpoint  | 2025-01-15       | 2025-04-15   | GET /api/v2/new-endpoint |

## Webhooks (if applicable)
<!-- Outbound webhooks the system fires -->

### lead.created
- **Fires when:** A new lead is created
- **Payload:**
  ```json
  { "event": "lead.created", "data": { "id": 1, "email": "..." }, "timestamp": "..." }
  ```
- **Retry policy:** 3 retries with exponential backoff

## API Versioning Strategy
- Current version: v1
- Versioning method: [URL path / Header / Query param]
- Deprecation policy: [e.g., 90-day notice before removal]
```

### When to Update API_REFERENCE.md

- **New endpoint added** → Add full entry with all fields
- **Endpoint modified** → Update params, responses, notes
- **Endpoint deprecated** → Move to Deprecated section with dates
- **Endpoint removed** → Remove from main catalog, keep in Deprecated for 1 version
- **Permission changed** → Update auth/permission fields
- **Schema changed** → Update request/response examples
- **`//update-api` called** → Full scan and sync

---

## 🔌 INTEGRATIONS REGISTRY (docs/INTEGRATIONS.md)

> Tracks every external service the project connects to — APIs, webhooks, OAuth, SDKs.

### Format

```markdown
# [PROJECT_NAME] — Integrations Registry

> Last updated: [date]

## Active Integrations

### INT-001: [Service Name] (e.g., Stripe, SendGrid, Twilio)
- **Purpose:** [Why we use this — e.g., "Payment processing"]
- **Type:** REST API / SDK / Webhook / OAuth
- **Status:** ✅ Active / ⚠️ Degraded / ❌ Down / 🔨 In Development
- **Adapter file:** [e.g., backend/integrations/stripe/adapter.py]
- **Interface:** [e.g., backend/integrations/stripe/interface.py]
- **Config/Env vars:** `STRIPE_API_KEY`, `STRIPE_WEBHOOK_SECRET`
- **Rate limits:** [e.g., 100 req/sec]
- **Fallback behavior:** [What happens if this service is down]
- **Docs:** [Link to their API docs]
- **Last verified:** [Date we last confirmed this works]
- **Endpoints used:**
  - `POST /v1/charges` — Create payment
  - `GET /v1/charges/{id}` — Get payment status
- **Webhook events consumed:**
  - `payment_intent.succeeded` → triggers [internal action]
- **Notes:** [Quirks, known issues, migration plans]

## Planned Integrations
### INT-XXX: [Service Name]
- **Purpose:** [Why we want this]
- **Blocked by:** [Dependency or decision needed]
- **Target phase:** [Which ROADMAP phase]

## Retired Integrations
### INT-000: [Service Name] — Retired [date]
- **Replaced by:** [INT-XXX or "Removed"]
- **Reason:** [Why it was retired]
```

---

## 🗺️ ROADMAP TRACKING (docs/ROADMAP.md)

### Format

```markdown
# [PROJECT_NAME] — Roadmap & Feature Tracker

> Last updated: [date]

## Build Metrics
| Metric              | Count |
|---------------------|-------|
| Backend endpoints   | N     |
| Database models     | N     |
| Frontend pages      | N     |
| Shared components   | N     |
| Test coverage       | N%    |
| Active migrations   | N     |
| External integrations | N   |

## Phase Overview
| Phase | Name                    | Status      | Progress |
|-------|-------------------------|-------------|----------|
| 1     | [Foundation]            | ✅ Complete | 100%     |
| 2     | [Core Features]         | 🔨 Active  | 60%      |
| 3     | [Advanced Features]     | ⬜ Planned  | 0%       |

## Phase 2 — [Core Features] (Active)

### ✅ Completed
- [x] T-201: [Task name] — [brief description]
- [x] T-202: [Task name] — [brief description]

### 🔨 In Progress
- [ ] T-203: [Task name] — [brief description]
  - Status: [details of where this stands]

### ⬜ Planned
- [ ] T-204: [Task name] — [brief description]
- [ ] T-205: [Task name] — [brief description]

### 🔒 Blocked
- [ ] T-206: [Task name] — Blocked by [B-XXX in RISKS.md]

### ⏸️ Deferred
- [ ] T-207: [Task name] — Deferred because [reason]
```

### Status Icons
```
✅ Done          — Completed and verified
🔨 In Progress   — Currently being built
⬜ Planned       — Queued, not started
🔒 Blocked       — Cannot proceed (see RISKS.md)
📐 Designed      — Spec complete, not built
⏸️ Deferred      — Intentionally postponed
```

---

## 🐛 BUG TRACKER (docs/BUGS.md)

### Format

```markdown
# [PROJECT_NAME] — Bug Tracker

> Last updated: [date]
> Open: [N] | Critical: [N] | High: [N] | Medium: [N] | Low: [N]

## Open Bugs

### BUG-001: [Short descriptive title]
- **Severity:** Critical / High / Medium / Low
- **Status:** Open / Investigating / Fix In Progress / In Review
- **Found:** [Date] — [Where/how it was discovered]
- **Component:** [backend / frontend / database / integration / infra]
- **Reproduction Steps:**
  1. [Step 1]
  2. [Step 2]
  3. [Expected vs actual behavior]
- **Root Cause:** [Known / Investigating / Unknown]
- **Assigned Fix:** [Task ID if linked to ROADMAP, or "Unplanned"]
- **Related Files:** [Files likely involved]
- **Related Endpoints:** [API endpoints affected, if any]
- **Workaround:** [Temporary fix if any, or "None"]

## Resolved Bugs (keep for history)

### BUG-000: [Title] — RESOLVED [date]
- **Resolution:** [What fixed it]
- **Regression test:** [Test added? Y/N + location]
- **Fix commit/PR:** [reference if available]
```

### Severity Definitions
```
Critical — App is broken, data at risk, or blocking all users
High     — Major feature broken, significant user impact, no workaround
Medium   — Feature partially broken, workaround exists
Low      — Cosmetic, minor UX issue, edge case
```

---

## ⚠️ RISKS & BLOCKERS (docs/RISKS.md)

### Format

```markdown
# [PROJECT_NAME] — Risks, Blockers & Dependencies

> Last updated: [date]

## Active Blockers

### B-001: [Short title]
- **Blocking:** [Task IDs from ROADMAP.md]
- **Description:** [What the problem is]
- **Root cause:** [Why it's blocked]
- **Unblock action:** [Specific step to resolve]
- **Owner:** dev / user / external / third-party
- **Priority:** Critical / High / Medium
- **Raised:** [Date]

## Risks (not blocking yet)

### R-001: [Short title]
- **Affects:** [Feature / phase]
- **Description:** [What could go wrong]
- **Likelihood:** High / Medium / Low
- **Impact:** High / Medium / Low
- **Mitigation:** [Prevention strategy]

## Technical Debt Register
<!-- Items that work but need improvement — tracked to prevent accumulation -->

### TD-001: [Short title]
- **Location:** [File(s) affected]
- **Issue:** [What's wrong with current approach]
- **Risk if not addressed:** [What could happen]
- **Estimated effort:** S / M / L
- **Target phase:** [When to address]

## Resolved (keep for history)

### B-000: [Title] — RESOLVED [date]
- **Resolution:** [How it was unblocked]
```

---

## 🧪 QA & TESTING (docs/QA.md)

### Format

```markdown
# [PROJECT_NAME] — QA & Testing Tracker

> Last updated: [date]

## Testing Strategy
- **Unit tests:** [framework, location, run command]
- **Integration tests:** [framework, location, run command]
- **E2E tests:** [framework, location, run command]
- **Manual test protocol:** [When and how manual testing is done]

## Coverage Summary
| Component           | Unit | Integration | E2E  | Manual | Notes            |
|---------------------|------|-------------|------|--------|------------------|
| Auth                | ✅   | ✅          | ⬜   | ✅     |                  |
| [Feature A]         | ✅   | ⬜          | ⬜   | ⬜     | Needs int. tests |
| [Feature B]         | ⬜   | ⬜          | ⬜   | ⬜     | Not started      |

## Pending Test Cases
### TC-001: [Test scenario name]
- **Feature:** [What feature this validates]
- **Type:** Unit / Integration / E2E / Manual
- **Priority:** Critical / High / Medium / Low
- **Steps:**
  1. [Setup]
  2. [Action]
  3. [Expected result]
- **Status:** ⬜ Not written / 🔨 In progress / ✅ Written / ✅ Passing / ❌ Failing

## Regression Tests (added from bug fixes)
- BUG-001 → TC-XXX: [Brief description of regression test]

## API Endpoint Test Matrix
<!-- Every endpoint should have at least happy path + auth + validation tests -->
| Endpoint               | Happy Path | Auth Check | Validation | Edge Cases | Notes |
|------------------------|------------|------------|------------|------------|-------|
| POST /api/auth/login   | ✅         | N/A        | ✅         | ✅         |       |
| GET /api/leads         | ✅         | ✅         | ⬜         | ⬜         |       |
```

---

## 📋 ARCHITECTURE DECISION RECORDS (docs/DECISIONS.md)

### Format

```markdown
# [PROJECT_NAME] — Architecture Decision Records

> Record of significant technical decisions and their reasoning.

## ADR-001: [Decision Title]
- **Date:** [date]
- **Status:** Accepted / Superseded by ADR-XXX / Deprecated
- **Context:** [What situation prompted this decision]
- **Options Considered:**
  - Option A: [description] — Pro: [x], Con: [y]
  - Option B: [description] — Pro: [x], Con: [y]
- **Decision:** [What we chose]
- **Rationale:** [Why — the key reasoning]
- **Consequences:** [What this means going forward, trade-offs accepted]
```

---

## 🏗️ ARCHITECTURE & DESIGN PRINCIPLES

### Object-Oriented & Modular Design (Non-Negotiable)

**Every piece of code must follow these principles. Monolithic code is never acceptable.**

1. **Single Responsibility Principle (SRP)**
   - Each class, module, service, and function does ONE thing well
   - If a file exceeds ~300 lines, it likely needs decomposition
   - Services handle business logic; routes handle HTTP; models handle data

2. **Open/Closed Principle (OCP)**
   - Open for extension, closed for modification
   - Use interfaces/abstract classes, strategy patterns, plugin architectures
   - New features should extend the system, not rewrite it

3. **Dependency Inversion Principle (DIP)**
   - Depend on abstractions, not concretions
   - Use dependency injection for services, database clients, external APIs
   - Every external integration should have an interface/adapter layer

4. **Interface Segregation**
   - Small, focused interfaces over large generic ones
   - Clients should not depend on methods they don't use

5. **Liskov Substitution**
   - Subclasses/implementations must be substitutable for their base types
   - No surprise behavior changes in derived classes

### Architecture Patterns (Always Apply)

```
LAYERED ARCHITECTURE (required for every feature):

┌─────────────────────────────────────┐
│  Routes / Controllers               │  ← HTTP handling, validation, auth
├─────────────────────────────────────┤
│  Services / Use Cases               │  ← Business logic, orchestration
├─────────────────────────────────────┤
│  Repositories / Data Access         │  ← Database queries, caching
├─────────────────────────────────────┤
│  Models / Entities                  │  ← Data structures, schemas
├─────────────────────────────────────┤
│  Integrations / Adapters            │  ← External APIs, third-party services
└─────────────────────────────────────┘

RULES:
- Routes NEVER contain business logic or direct DB queries
- Services NEVER import from routes
- Models NEVER import from services
- Integrations are ALWAYS behind an adapter interface
- Each layer only talks to the layer directly below it
```

### Future-Proof Design Checklist

Before implementing ANY feature, evaluate:

- [ ] **Scalability:** Will this work with 10x the current load?
- [ ] **Modularity:** Can this component be replaced without touching others?
- [ ] **Configuration over code:** Is behavior configurable (env vars, feature flags, settings)?
- [ ] **Migration path:** Can this be migrated to a different DB/framework/provider?
- [ ] **Monitoring:** Can I observe this in production (logging, metrics, health checks)?
- [ ] **Testability:** Can I unit test this in isolation?
- [ ] **Multi-tenancy:** Is data properly scoped and isolated?
- [ ] **Backwards compatibility:** Does this break existing functionality?
- [ ] **Industry flexibility:** Is this domain-specific or generic?
- [ ] **Integration readiness:** Can external systems hook into this (webhooks, APIs, events)?

### Code Organization Rules

```
FEATURE-BASED STRUCTURE (preferred):

backend/
├── features/
│   ├── auth/
│   │   ├── routes.py          ← HTTP endpoints
│   │   ├── services.py        ← Business logic
│   │   ├── models.py          ← Database models
│   │   ├── schemas.py         ← Request/response schemas
│   │   ├── repository.py      ← Data access layer
│   │   └── tests/
│   ├── leads/
│   │   ├── routes.py
│   │   ├── services.py
│   │   └── ...
│   └── ...
├── core/                      ← Shared utilities, base classes
│   ├── database.py
│   ├── config.py
│   ├── exceptions.py
│   └── middleware.py
├── integrations/              ← External service adapters
│   ├── base.py                ← Abstract integration interface
│   ├── email/
│   ├── sms/
│   └── crm/
└── main.py

KEY RULES:
- Feature folders are self-contained — moving or removing one doesn't break others
- Shared code lives in core/ — never in a specific feature folder
- Integrations are always behind interfaces for easy swapping
- Tests live next to what they test
```

### Speed, Efficiency & Observability

1. **Async by default** — All IO operations (DB, HTTP, file) must be async
2. **Connection pooling** — Database and HTTP connections must be pooled
3. **Pagination** — Every list endpoint must support pagination (never return unbounded results)
4. **Indexing** — Every column used in WHERE, JOIN, or ORDER BY gets an index
5. **Caching strategy** — Define cache layers (in-memory, Redis, CDN) per use case
6. **Structured logging** — Use structured logs (JSON format) with correlation IDs
7. **Health checks** — Every service exposes a `/health` endpoint
8. **Error tracking** — Errors are caught, logged with context, and surfaced (never silently swallowed)
9. **Metrics** — Track request latency, error rates, DB query times
10. **Graceful degradation** — If an integration fails, the core app still works

### Migration Safety Rules

- All migrations MUST be reversible (include downgrade/rollback)
- Backfill existing data when adding NOT NULL columns
- Never drop columns until all code is migrated off them
- Test migrations on a copy before running on production data
- Large data migrations should be batched, not run in a single transaction
- Always document migration in CHANGELOG.md

---

## 🔄 CORE BEHAVIOR RULES

### 1. Always Plan Before Building

**Never start coding without presenting a plan first.**

```
PLAN: [Task Name]

What I'll do:
  1. [Step] — [Why]
  2. [Step] — [Why]
  3. [Step] — [Why]

Files I'll touch:
  - path/to/file.py (modify — reason)
  - path/to/new_file.py (CREATE — purpose)

Architecture impact:
  - [New service/model/endpoint being introduced]
  - [Existing contracts affected]

What this achieves:
  - [Capability added or problem solved]

What this does NOT do (deferred scope):
  - [Related items intentionally left for later]

Risks / Blockers:
  - [Dependencies, migration concerns, unknowns]

Documents to update after:
  - [List of docs/ files that will need updating]

Shall I proceed?
```

**Wait for confirmation.** Quick fixes (typo, single-line) may proceed directly but still require a post-change explanation.

### 2. Always Explain What You Did

After every change:

```
DONE: [Task Name]

What changed:
  - [File]: [What + why]
  - [File]: [What + why]

What's now possible:
  - [New capability or fixed behavior]

What to test:
  - [Verification steps]

Documents updated:
  - [List of docs/ files updated]

What's next:
  - [Logical follow-up task]
```

### 3. Always Ask When Uncertain

For ambiguous tasks, present options:

```
QUESTION: [Topic]

Option A: [Description]
  Pro: [benefit]
  Con: [trade-off]

Option B: [Description]
  Pro: [benefit]
  Con: [trade-off]

My recommendation: [A or B] because [reason].
Which direction?
```

**Never guess on:** architectural decisions, data model changes, security-related code,
anything affecting multiple files or modules.

### 4. Protect Existing Functionality

Before modifying existing code:
- Read the current implementation
- Understand what depends on it
- Preserve backward compatibility unless explicitly told to break it
- Flag breaking changes:

```
CAUTION: This change affects [X existing feature].

Current behavior: [what happens now]
New behavior: [what will happen after]
Risk: [what could break]
Mitigation: [how I'll prevent breakage]
```

### 5. Write Production-Quality Code

- Proper error handling on every endpoint/function
- Input validation on all user-facing inputs
- Auth/permission checks on every route
- Loading, error, and empty states on every frontend view
- No hardcoded values — use configuration
- No TODO comments without a corresponding ROADMAP entry
- Follow existing patterns — consistency over cleverness
- Every new file follows project language rules (see PROJECT IDENTITY)

### 6. Think About Growth at Every Step

Before implementing, ask:
- Will this work when the next planned feature is added?
- Will this work for a different industry or use case?
- Is this configurable per tenant/organization/user?
- Am I building a wall or a door?
- Can this be monitored and debugged in production?
- Can this be tested in isolation?

If something is domain-specific, make it configurable (settings, feature flags, seed data) — never hardcoded if/else branches.

---

## 🎯 NATURAL LANGUAGE COMMAND RESPONSES

In addition to `//slash-commands`, the agent also responds to natural language keywords:

### "roadmap" / "where are we" / "what's the plan"
→ Read `docs/ROADMAP.md` and present: current phase, progress, next priorities, recommendation.

### "blockers" / "what's blocked" / "risks"
→ Read `docs/RISKS.md` and present: active blockers, unblock actions, risks, recommendation.

### "bugs" / "open issues" / "what's broken"
→ Read `docs/BUGS.md` and present: open bugs by severity, oldest unresolved, recommendation.

### "qa" / "testing" / "what needs testing"
→ Read `docs/QA.md` and present: coverage gaps, pending test cases, highest priority tests.

### "status" / "build stats"
→ Read `docs/ROADMAP.md` and present: metrics table, completion percentages, recent changes.

### "context" / "where did we leave off" / "what do you know"
→ Read `docs/CONTEXT.md` and present: current state, recent decisions, next priority.

### "decisions" / "why did we" / "ADRs"
→ Read `docs/DECISIONS.md` and present: relevant decision records.

### "api" / "endpoints" / "what endpoints exist"
→ Read `docs/API_REFERENCE.md` and present: endpoint count, groups, recent additions.

### "integrations" / "external services" / "what's connected"
→ Read `docs/INTEGRATIONS.md` and present: active integrations, status, any issues.

### "tech debt" / "what needs cleanup"
→ Read `docs/RISKS.md` Technical Debt Register and present: items by effort and risk.

### "what should we build next" / "priorities"
→ Read ROADMAP + RISKS + BUGS, then recommend the highest-impact unblocked task.

### "fix [something]"
→ Reproduce → root cause → plan → fix → update BUGS.md + QA.md + CHANGELOG.md.

### "add [feature]" / "build [something]"
→ Check codex/ for spec → check ROADMAP for fit → check RISKS for blockers → plan → build → update all docs.

---

## 🚀 SESSION MANAGEMENT

### Session Start Checklist

At the beginning of every work session, before doing anything else:

1. **Read `docs/CONTEXT.md`** — Know where we left off
2. **Read `docs/ROADMAP.md`** — Know what phase we're in
3. **Read `docs/RISKS.md`** — Know what's blocked
4. **Read `docs/BUGS.md`** — Know what's broken
5. **Reset prompt counter** to 0
6. **Greet with status:**

```
Session Start — [PROJECT_NAME]

Current phase: [Phase X — Name] ([N]% complete)
Active blockers: [count] ([brief list or "None"])
Open bugs: [count by severity or "None"]
Last completed: [most recent task]
Next priority: [recommended task + why]

Ready to continue, or want to work on something else?
```

### Session End / Compaction Protocol

When a session is ending, conversation is being compacted, or `//handoff` is called:

1. **Update `docs/CONTEXT.md`** with:
   - Everything accomplished this session
   - Decisions made and their reasoning
   - Current state of in-progress work
   - Any user preferences or clarifications learned
   - Next steps and priorities
   - Final prompt counter value
2. **Update `docs/ROADMAP.md`** with any task status changes
3. **Update `docs/BUGS.md`** with any new or resolved bugs
4. **Update `docs/API_REFERENCE.md`** if any endpoints were added/changed
5. **Update `docs/CHANGELOG.md`** with what changed
6. **Confirm:** "Context saved. All docs current. Ready for next session or platform handoff."

### Platform Handoff Protocol (`//handoff`)

If moving to a different AI session or platform:

1. Run the full Session End protocol above
2. Ensure ALL docs/ files are current and consistent with each other
3. Add a handoff note to CONTEXT.md with timestamp
4. The new session just needs: `//catch-up` or "Read the CLAUDE.md and docs/ folder, then tell me where we are."

---

## 🚫 ERROR RECOVERY

When something goes wrong:

1. **Don't panic-fix.** Stop, explain what happened and why.
2. **Roll back if needed.** Use version control to revert cleanly.
3. **Understand root cause** before attempting a fix.
4. **If a fix cascades** (touches 3+ files), present it as a plan first.
5. **Log it:** Add to BUGS.md if it's a real bug, RISKS.md if it reveals a risk.
6. **If stuck:**

```
STUCK: [What happened]

I tried: [approach]
It failed because: [reason]
Options:
  A. [Alternative approach]
  B. [Ask user for input]
  C. [Defer and work on something else]

What would you like to do?
```

---

## ❌ WHAT NOT TO DO

- **Don't build without a plan** — even for "quick" changes
- **Don't write monolithic code** — decompose into layers and modules ALWAYS
- **Don't skip permission/auth checks** on new endpoints
- **Don't use loose types** (no `any` in TS, no `dict` without type hints in Python)
- **Don't leave the codebase broken** — every commit should be functional
- **Don't forget to update docs/** — they are living documents, not write-once artifacts
- **Don't assume — ask** — if requirements are ambiguous, clarify before building
- **Don't silently swallow errors** — log them, surface them, handle them
- **Don't hardcode values** — use config, env vars, feature flags
- **Don't rebuild what works** — extend, enhance, evolve
- **Don't skip tests** — every new feature needs at least a basic test plan in QA.md
- **Don't make architectural decisions without recording them** in DECISIONS.md
- **Don't ignore the context document** — CONTEXT.md is your memory, treat it as critical
- **Don't write tightly coupled code** — if swapping one component breaks everything, refactor
- **Don't create database models without migrations** — ever
- **Don't deploy without a rollback plan** — migrations, feature flags, or revert strategy
- **Don't add API endpoints without updating API_REFERENCE.md** — every endpoint must be documented
- **Don't add integrations without updating INTEGRATIONS.md** — every external connection must be tracked
- **Don't let technical debt accumulate silently** — log it in RISKS.md Tech Debt Register

---

## 📊 SCAFFOLD & ONBOARD

### `//scaffold` — For New Projects

Creates the docs/ structure based on `PROJECT_SCALE`:

**🔴 FULL:** All 11 documents + codex/ folder
**🟡 STANDARD:** Core 6 documents + codex/ folder
**🟢 LITE:** 2 documents (CONTEXT.md + CHANGELOG.md)

Each file is populated with template headers customized with PROJECT_NAME.

After scaffolding, confirm:
```
✅ Project docs scaffolded for [PROJECT_NAME] (scale: [FULL/STANDARD/LITE]).
Created [N] documents in docs/.
Run //catch-up anytime to get a full status briefing.
```

### `//onboard` — For Existing Projects

See the full Existing Project Onboarding section above. The agent:
1. Scans the codebase (read-only)
2. Performs gap analysis against the project scale requirements
3. Creates missing docs by analyzing actual code (with approval)
4. Logs tech debt and recommendations
5. NEVER modifies existing code during onboarding

---

## 📋 QUICK REFERENCE — All Slash Commands

| Command                    | Category     | Description                                              |
|----------------------------|--------------|----------------------------------------------------------|
| `//catch-up`               | Context      | New session onboarding — read all docs, present status   |
| `//context-backup`         | Context      | Immediate full context save to CONTEXT.md                |
| `//where-are-we`           | Context      | Quick status check                                       |
| `//handoff`                | Context      | Full platform handoff — update everything for migration  |
| `//update-roadmap`         | Planning     | Sync ROADMAP.md with actual project state                |
| `//new-feature [name]`     | Planning     | Full new feature analysis + roadmap integration plan     |
| `//priorities`             | Planning     | Top 3 recommended tasks with reasoning                   |
| `//update-documents`       | Docs         | Full documentation sync — find and fix all stale docs    |
| `//update-api`             | Docs         | Sync API_REFERENCE.md with actual endpoints              |
| `//update-models`          | Docs         | Sync DATA_MODELS.md with actual models                   |
| `//update-architecture`    | Docs         | Update ARCHITECTURE.md to reflect current state          |
| `//log-bug [desc]`         | QA           | Create new bug entry in BUGS.md                          |
| `//bugs`                   | QA           | Show open bugs by severity                               |
| `//qa-status`              | QA           | Show test coverage gaps and pending tests                |
| `//fix [desc]`             | Build        | Bug fix pipeline with doc updates                        |
| `//build [feature]`        | Build        | Feature build pipeline with doc updates                  |
| `//refactor [target]`      | Build        | Refactor pipeline with before/after analysis             |
| `//scaffold`               | Setup        | Create full docs/ structure with templates (new project) |
| `//onboard`                | Setup        | Analyze existing project, create missing docs, gap analysis |
| `//health-check`           | Maintenance  | Project health diagnostic — missing docs, gaps, concerns |
| `//dependency-audit`       | Maintenance  | Review dependencies for issues and security              |
| `//git-backup`             | Git          | Emergency WIP commit + push to remote                    |
| `//git-status`             | Git          | Current branch, changes, ahead/behind remote             |
| `//git-commit [msg]`       | Git          | Stage + commit with message (or auto-suggest)            |
| `//git-push`               | Git          | Push current branch to remote                            |
| `//git-branch [name]`      | Git          | Create + switch to new feature branch                    |
| `//git-merge`              | Git          | Merge current branch into default branch                 |
| `//git-tag [version]`      | Git          | Tag release version based on CHANGELOG                   |
| `//get-bugs`               | Git          | Pull bugs/alerts/issues from remote platform, analyze, update docs |
| `//check-prs`              | Git          | PR health check — failing, stale, conflicting, ready to merge |
