# 🤖 CLAUDE.md — Universal Development Agent Configuration

> **One file. Any project. Full dev pipeline management.**

A comprehensive, project-agnostic configuration file that transforms AI coding agents into senior development partners — with built-in project tracking, documentation management, bug tracking, QA, architecture governance, Git integration, and persistent context memory across sessions.

Drop it into any project. Tell the agent `//catch-up`. Start building.

---

## 📋 Table of Contents

- [What Is This?](#what-is-this)
- [Why Does This Exist?](#why-does-this-exist)
- [Quick Start](#-quick-start)
- [Features](#-features)
- [Slash Commands](#-slash-commands--29-commands)
- [Project Scale System](#-project-scale--adaptive-complexity)
- [Documentation Framework](#-documentation-framework--11-living-documents)
- [Architecture Philosophy](#-architecture-philosophy)
- [Git & Platform Integration](#-git--platform-integration)
- [Context Memory System](#-context-memory--never-lose-progress)
- [Files In This Repo](#-files-in-this-repo)
- [Installation](#-installation)
- [Configuration](#-configuration)
- [FAQ](#-faq)
- [Version History](#-version-history)
- [Contributing](#-contributing)
- [License](#-license)

---

## What Is This?

`CLAUDE.md` is a configuration file that gives AI development agents a complete operating system for managing software projects. Think of it as a **job description + playbook + dev pipeline** — all in one markdown file.

When an AI agent reads this file, it knows how to:

- 📋 **Plan before building** — present structured plans with file lists, risks, and scope
- 📝 **Track everything** — roadmap, bugs, risks, QA, architecture decisions, API endpoints
- 🧠 **Remember across sessions** — persistent context that survives conversation compaction
- 🔀 **Manage Git workflows** — branching, commits, tags, remote sync
- 🌍 **Pull from GitHub** — ingest Dependabot alerts, issues, CI failures, PR status
- 📐 **Enforce architecture** — SOLID principles, layered design, no monolithic code
- 📊 **Scale to your project** — from a 50-line script to a full-stack SaaS platform

**It doesn't run code.** It's pure instructions — a 1,700-line playbook that teaches the AI agent how to behave professionally across any project.

---

## Why Does This Exist?

Without this, every AI coding session starts from zero. You explain the same architecture, repeat the same rules, lose context when conversations compact, and forget to update documentation.

With this, every session starts informed. The agent reads your CLAUDE.md, checks your docs, and picks up exactly where you left off — even if you switch platforms, sessions, or AI providers.

**Problems this solves:**

| Problem | How CLAUDE.md Solves It |
|---------|------------------------|
| AI forgets context between sessions | `CONTEXT.md` persists memory + auto-backup every N prompts |
| Documentation gets stale | Agent updates all relevant docs after every change |
| No bug tracking in small projects | `BUGS.md` with severity, repro steps, resolution history |
| Architecture degrades over time | SOLID principles enforced, tech debt tracked, ADRs recorded |
| Can't pick up where I left off | `//catch-up` reads all docs and presents full status |
| Losing GitHub issues/alerts | `//get-bugs` pulls Dependabot, Issues, CI status into local docs |
| Overkill process for small scripts | `PROJECT_SCALE: LITE` reduces to just 2 docs + simple workflow |

---

## 🚀 Quick Start

### New Project

```bash
# 1. Copy CLAUDE.md to your project root
cp CLAUDE.md /path/to/your/project/

# 2. Edit the PROJECT IDENTITY section (first ~80 lines)
#    - Set PROJECT_NAME, TECH_STACK, PROJECT_SCALE, GIT config

# 3. Start your AI agent and tell it:
//scaffold
#    → Creates the full docs/ structure with templates

# 4. Start building
//build user authentication
```

### Existing Project

```bash
# 1. Copy CLAUDE.md to your project root
cp CLAUDE.md /path/to/your/existing-project/

# 2. Edit the PROJECT IDENTITY section

# 3. Start your AI agent and tell it:
//onboard
#    → Scans codebase (read-only), gap analysis, creates missing docs
#    → NEVER modifies your existing code

# 4. Continue working
//catch-up
```

---

## ✨ Features

### 🎛️ Adaptive Project Scale

One config works for everything — the agent adjusts its behavior based on complexity:

| | 🔴 FULL | 🟡 STANDARD | 🟢 LITE |
|---|---|---|---|
| **Use for** | SaaS, multi-service, team projects | Internal tools, medium APIs | Scripts, CLIs, prototypes |
| **Required docs** | All 11 | Core 6 | Just 2 |
| **Planning rigor** | Formal PLAN block always | Brief plans OK | Only if 3+ files |
| **Architecture** | Strict SOLID + layered | Encouraged | Functional/procedural OK |
| **Auto-backup** | Every 10 prompts | Every 15 prompts | Every 25 prompts |
| **Git workflow** | Feature branches | Feature branches OK | Trunk (main only) |

### 🧠 Persistent Context Memory

The `CONTEXT.md` system ensures nothing is lost:

- **Auto-backup** at regular intervals (not just on compaction)
- **Session handoff** — move between AI platforms seamlessly
- **Rolling decision log** — why things were built the way they were
- **Onboarding instructions** — any new agent can get up to speed instantly

### 🌍 Remote Platform Integration

Pull bugs and alerts directly from GitHub/GitLab/Bitbucket:

- **Dependabot alerts** — security vulnerabilities with CVSS severity
- **Dependabot PRs** — pending patches ready to merge
- **GitHub Issues** — bug-labeled issues ingested into local tracking
- **CI/CD status** — failing pipelines flagged as infra bugs
- **Security advisories** — repo-level security concerns
- **PR health** — failing checks, merge conflicts, stale PRs

### 📝 Living Documentation (11 Documents)

Every doc updates automatically as the project evolves:

| Document | Purpose |
|----------|---------|
| `CONTEXT.md` | Session memory — survives compaction and platform switches |
| `CHANGELOG.md` | Version history — what changed and when |
| `ROADMAP.md` | Feature tracker with phases, tasks, metrics |
| `BUGS.md` | Bug tracker with severity, repro steps, resolution |
| `ARCHITECTURE.md` | System design, models, services, data flow |
| `API_REFERENCE.md` | Every endpoint: method, path, auth, schemas, examples |
| `RISKS.md` | Blockers, risks, tech debt register |
| `QA.md` | Test plans, coverage matrix, pending test cases |
| `DATA_MODELS.md` | Entity relationships, field types, constraints |
| `DECISIONS.md` | Architecture Decision Records (ADRs) |
| `INTEGRATIONS.md` | External services, adapters, rate limits, fallbacks |

---

## ⚡ Slash Commands — 29 Commands

Type these in your AI agent for immediate action:

### Context & Memory
| Command | Description |
|---------|-------------|
| `//catch-up` | New session onboarding — reads all docs, presents full status |
| `//context-backup` | Immediate context save to CONTEXT.md |
| `//where-are-we` | Quick status check |
| `//handoff` | Full platform handoff — updates everything for migration |

### Planning & Roadmap
| Command | Description |
|---------|-------------|
| `//update-roadmap` | Sync ROADMAP.md with actual project state |
| `//new-feature [name]` | Full analysis: architecture fit, blockers, build plan, scope |
| `//priorities` | Top 3 highest-impact unblocked tasks |

### Documentation
| Command | Description |
|---------|-------------|
| `//update-documents` | Full sync — find and fix all stale docs |
| `//update-api` | Sync API_REFERENCE.md with actual endpoints |
| `//update-models` | Sync DATA_MODELS.md with actual models |
| `//update-architecture` | Update ARCHITECTURE.md to reflect current state |

### Bug & QA
| Command | Description |
|---------|-------------|
| `//log-bug [desc]` | Create new bug entry in BUGS.md |
| `//bugs` | Show open bugs by severity |
| `//qa-status` | Show test coverage gaps and pending tests |

### Build & Fix
| Command | Description |
|---------|-------------|
| `//fix [desc]` | Bug fix pipeline: reproduce → root cause → plan → fix → docs |
| `//build [feature]` | Feature pipeline: spec → roadmap → plan → build → docs |
| `//refactor [target]` | Refactor pipeline with before/after analysis |

### Git & Version Control
| Command | Description |
|---------|-------------|
| `//git-backup` | Emergency WIP commit + push |
| `//git-status` | Branch, uncommitted changes, ahead/behind remote |
| `//git-commit [msg]` | Stage + commit (or auto-suggest message) |
| `//git-push` | Push current branch to remote |
| `//git-branch [name]` | Create + switch to feature branch |
| `//git-merge` | Merge current branch into default branch |
| `//git-tag [version]` | Tag release version |
| `//get-bugs` | Pull bugs/alerts from GitHub — Dependabot, Issues, CI, security |
| `//check-prs` | PR health check — failing, stale, conflicts, ready to merge |

### Project Setup & Maintenance
| Command | Description |
|---------|-------------|
| `//scaffold` | Create docs/ structure with templates (new projects) |
| `//onboard` | Analyze existing project, gap analysis, create missing docs |
| `//health-check` | Project health diagnostic |
| `//dependency-audit` | Review dependencies for security and updates |

---

## 📐 Project Scale — Adaptive Complexity

Set `PROJECT_SCALE` in the config to control how much process the agent applies:

```yaml
# In CLAUDE.md → PROJECT IDENTITY section:
PROJECT_SCALE: "FULL"      # Full-stack SaaS, team projects
PROJECT_SCALE: "STANDARD"  # Medium apps, APIs, solo projects
PROJECT_SCALE: "LITE"      # Scripts, CLIs, prototypes
```

**Upgrading is seamless:** Change LITE → STANDARD → FULL anytime, then run `//onboard` to scaffold the additional docs. Nothing breaks.

---

## 📁 Documentation Framework — 11 Living Documents

The agent maintains all docs in a `docs/` directory at your project root:

```
your-project/
├── CLAUDE.md              ← Agent configuration (this file)
├── docs/
│   ├── CONTEXT.md         ← 🔴🟡🟢  Session memory
│   ├── CHANGELOG.md       ← 🔴🟡🟢  Version history
│   ├── ROADMAP.md         ← 🔴🟡     Feature tracker
│   ├── BUGS.md            ← 🔴🟡     Bug tracker
│   ├── ARCHITECTURE.md    ← 🔴🟡     System design
│   ├── API_REFERENCE.md   ← 🔴🟡     Endpoint catalog
│   ├── RISKS.md           ← 🔴       Blockers + risks + tech debt
│   ├── QA.md              ← 🔴       Test plans + coverage
│   ├── DATA_MODELS.md     ← 🔴       Entity relationships
│   ├── DECISIONS.md       ← 🔴       Architecture decisions
│   ├── INTEGRATIONS.md    ← 🔴       External services
│   └── codex/             ← 🔴🟡     Feature specifications
└── ...your code...

🔴 = Required for FULL  |  🟡 = Required for STANDARD  |  🟢 = Required for LITE
```

---

## 🏗️ Architecture Philosophy

The agent enforces these principles on every feature (strictness varies by scale):

- **SOLID Principles** — Single Responsibility, Open/Closed, Dependency Inversion, Interface Segregation, Liskov Substitution
- **Layered Architecture** — Routes → Services → Repositories → Models → Integrations
- **No Monolithic Code** — decompose into modules, max ~300 lines per file
- **Future-Proof Checklist** — scalability, modularity, configuration over code, migration paths, monitoring, testability
- **Async by Default** — all IO operations async with connection pooling
- **Observable Systems** — structured logging, health checks, error tracking, metrics

---

## 🔀 Git & Platform Integration

### Git Workflow
- **Conventional Commits** — `feat(scope): message`, `fix(scope): message`, etc.
- **Branch Naming** — `feature/T-{id}-{name}`, `fix/BUG-{id}-{name}`
- **Scale-Appropriate** — feature branches for FULL/STANDARD, trunk for LITE
- **Safety Net** — `//git-backup` for emergency WIP commits

### Remote Platform Scanning
Configure which sources to scan in `GIT.platform_features`:

```yaml
GIT:
  remote: "github"
  platform_features:
    issues: true              # GitHub Issues
    dependabot_alerts: true   # Security vulnerability alerts
    dependabot_prs: true      # Pending security/version PRs
    security_advisories: true # Repo-level advisories
    ci_status: true           # CI/CD pipeline failures
    pull_requests: true       # PR health monitoring
```

Then use `//get-bugs` to pull everything in, or `//check-prs` for PR-focused checks.

---

## 🧠 Context Memory — Never Lose Progress

The auto-backup system proactively saves context at regular intervals:

| Trigger | FULL | STANDARD | LITE |
|---------|------|----------|------|
| Silent backup | Every 10 prompts | Every 15 prompts | Every 25 prompts |
| Checkpoint message | Every 20 prompts | Every 30 prompts | Every 50 prompts |
| Full doc sync | Every 30 prompts | Every 45 prompts | On demand |
| Major milestone | Immediate | Immediate | Immediate |
| `//context-backup` | Immediate | Immediate | Immediate |
| Compaction detected | Emergency save | Emergency save | Emergency save |

**Platform handoff:** Run `//handoff`, then on the new platform just say `//catch-up` — the agent reads all docs and resumes exactly where you left off.

---

## 📂 Files In This Repo

```
├── CLAUDE.md                      ← The main configuration file (drop into any project)
├── CLAUDE_Cheat_Sheet_v2.2.0.pdf  ← 3-page printable quick reference
├── README.md                      ← You are here
├── LICENSE                        ← License file
└── examples/                      ← (coming soon) Example configs for common stacks
    ├── fastapi-react/
    ├── nextjs/
    ├── python-cli/
    └── express-vue/
```

---

## 💾 Installation

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/claude-md.git

# Copy CLAUDE.md into your project
cp claude-md/CLAUDE.md /path/to/your/project/

# Edit the PROJECT IDENTITY section (lines 17-85)
# Set: PROJECT_NAME, DESCRIPTION, TECH_STACK, PROJECT_SCALE, GIT config

# Start your AI agent in the project directory
# For Claude Code:
cd /path/to/your/project && claude

# Then tell it:
//scaffold    # New project
//onboard     # Existing project
```

### Compatibility

| Platform | How It Works |
|----------|-------------|
| **Claude Code** | Auto-detected — `CLAUDE.md` is a built-in feature. Highest instruction adherence. |
| **Claude.ai / Claude App** | Upload or paste as project instructions. Full functionality. |
| **Cursor** | Use as `.cursorrules` or reference in project settings. |
| **Windsurf** | Use as `.windsurfrules` or reference in project settings. |
| **ChatGPT / Other AI** | Paste into system prompt or upload as context. All concepts are platform-agnostic. |

---

## ⚙️ Configuration

The only section you edit per project is **PROJECT IDENTITY** (first ~85 lines):

```yaml
PROJECT_NAME: "MyApp"
DESCRIPTION: "E-commerce platform with inventory management"
INDUSTRY: "e-commerce"
MULTI_TENANT: true
PROJECT_SCALE: "FULL"       # FULL / STANDARD / LITE

TECH_STACK:
  backend: "FastAPI"
  frontend: "React + TypeScript"
  database: "PostgreSQL"
  # ... etc

GIT:
  enabled: true
  remote: "github"
  repo_url: "https://github.com/user/myapp"
  default_branch: "main"
  branching_strategy: "feature"
  commit_convention: "conventional"
  platform_features:
    issues: true
    dependabot_alerts: true
    dependabot_prs: true
    security_advisories: true
    ci_status: true
    pull_requests: true
```

Everything else in the file is universal — no edits needed.

---

## ❓ FAQ

**Q: Will this modify my existing code?**
No. CLAUDE.md is instructions only. It tells the agent *how* to work. The `//onboard` command specifically scans without touching code — it only creates `docs/` files.

**Q: Is 1,700 lines too long?**
For a reference playbook, no. For Claude Code specifically (where CLAUDE.md loads into every prompt), Anthropic recommends keeping it lean. A future version will include a trimmed ~150-line version that references the full playbook in `docs/`. For Claude.ai, Cursor, and other platforms, the full version works great.

**Q: Can I use this with non-Claude AI tools?**
Yes. The concepts (planning, doc tracking, slash commands, context memory) are universal. The slash command names are arbitrary — any AI that reads markdown instructions will follow them.

**Q: What if my project doesn't use Git?**
Set `GIT.enabled: false`. All Git-related commands gracefully skip. The rest of the system (docs, planning, QA, architecture) works independently.

**Q: Can I use this for multiple projects simultaneously?**
Yes. Each project gets its own copy of CLAUDE.md with its own PROJECT IDENTITY config. The `docs/` folder is project-specific.

**Q: What's the difference between `//scaffold` and `//onboard`?**
`//scaffold` creates empty templates for new projects. `//onboard` analyzes an existing codebase and creates docs pre-populated with what it discovers.

---

## 📜 Version History

| Version | Date | Highlights |
|---------|------|------------|
| **2.2.0** | 2025-02-26 | Git integration, `//get-bugs` + `//check-prs` (Dependabot/Issues/CI), project scale system (FULL/STANDARD/LITE), existing project `//onboard`, platform features config |
| **2.1.0** | 2025-02-26 | Slash command system (27 commands), auto-backup with prompt counter, API_REFERENCE.md, INTEGRATIONS.md, tech debt register, `//health-check`, `//dependency-audit` |
| **2.0.0** | 2025-02-26 | Universal rewrite — project-agnostic, CONTEXT.md memory system, BUGS.md, QA.md, DECISIONS.md, SOLID principles, layered architecture, future-proof design checklist |
| **1.0.0** | — | Original NexusLead-specific configuration (project-specific, not universal) |

---

## 🤝 Contributing

This is an evolving system. Ideas for improvement are welcome:

1. Fork the repo
2. Create a feature branch (`git checkout -b feature/your-idea`)
3. Make your changes
4. Submit a PR with a clear description of what you added and why

**Areas open for contribution:**
- Example configs for popular tech stacks (NextJS, Django, Spring Boot, etc.)
- Trimmed Claude Code-optimized version (~150 lines)
- Translations to other AI platforms' config formats (`.cursorrules`, `.windsurfrules`)
- Additional slash commands
- Integration with more platforms (Azure DevOps, Jira, Linear)

---

## 📄 License

MIT License — use it, modify it, share it. See [LICENSE](LICENSE) for details.

---

<p align="center">
  <b>Built with ❤️ by developers who got tired of repeating themselves to AI agents.</b>
  <br><br>
  <i>If this saved you time, give it a ⭐</i>
</p>
