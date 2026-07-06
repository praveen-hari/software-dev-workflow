# Software Development Workflow

Production-grade software development workflow for AI coding agents. Orchestrates a 6-phase lifecycle using [agent-skills](https://github.com/addyosmani/agent-skills) for engineering discipline and [Syncfusion UI Builders](https://www.syncfusion.com/explore/agentic-ui-builder/) for frontend generation across 7 frameworks.

```
  DEFINE          PLAN           BUILD          VERIFY         REVIEW          SHIP
 ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐
 │ Idea │ ───▶ │ Spec │ ───▶ │ Code │ ───▶ │ Test │ ───▶ │  QA  │ ───▶ │  Go  │
 │Refine│      │  PRD │      │ Impl │      │Debug │      │ Gate │      │ Live │
 └──────┘      └──────┘      └──────┘      └──────┘      └──────┘      └──────┘
  /spec          /plan          /build        /test         /review       /ship
```

## What This Package Does

- **Orchestrates 24 agent-skills** through a structured 6-phase lifecycle
- **Auto-detects project stack** and routes UI work to the correct Syncfusion UI Builder
- **Enforces engineering discipline**: spec before code, TDD, code review, security hardening
- **Provides 6 slash commands** (`/spec`, `/plan`, `/build`, `/test`, `/review`, `/ship`)
- **Includes 4 specialist agents**: code reviewer, security auditor, test engineer, workflow orchestrator
- **Works for any tech stack** — the engineering workflow is stack-agnostic

## Supported Frameworks (UI Generation)

| Framework | UI Builder | Stack |
|-----------|-----------|-------|
| React | `syncfusion/react-ui-builder` | Web |
| Angular | `syncfusion/angular-ui-builder` | Web |
| Blazor | `syncfusion/blazor-ui-builder` | Web (.NET) |
| .NET MAUI | `syncfusion/maui-ui-builder` | Cross-platform |
| WPF | `syncfusion/wpf-ui-builder` | Windows Desktop |
| WinForms | `syncfusion/winforms-ui-builder` | Windows Desktop |
| WinUI | `syncfusion/winui-ui-builder` | Windows Desktop |

For non-Syncfusion stacks, the workflow uses `agent-skills` directly (incremental-implementation + frontend-ui-engineering).

## Prerequisites

- [APM (Agent Package Manager)](https://microsoft.github.io/apm/getting-started/installation/#quick-install-recommended)
- A supported AI agent or IDE (Code Studio, VS Code, Cursor, Claude Code, etc.)

## Installation

```bash
# Install for GitHub Copilot
apm install your-org/software-dev-workflow -t copilot

# Install for Claude Code
apm install your-org/software-dev-workflow -t claude

# Install for Cursor
apm install your-org/software-dev-workflow -t cursor

# Install for Codex
apm install your-org/software-dev-workflow -t codex
```

This automatically installs all dependencies:
- ✅ 24 agent-skills (engineering workflows)
- ✅ All 7 Syncfusion UI Builders (framework-specific frontend generation)

## Usage

### Slash Commands

| Command | Phase | What Happens |
|---------|-------|-------------|
| `/spec` | Define | Interview → Idea Refine → Write PRD specification |
| `/plan` | Plan | Break spec into vertical slices with acceptance criteria |
| `/build` | Build | Implement slice-by-slice with TDD + UI Builder routing |
| `/test` | Verify | Run tests, debug failures, verify at runtime |
| `/review` | Review | 5-axis code review + security audit + test coverage |
| `/ship` | Ship | Git workflow → CI/CD → Deploy → Monitor |

### Workflow Routes

Not every task needs the full lifecycle:

| Task Type | Route |
|-----------|-------|
| New feature | `/spec` → `/plan` → `/build` → `/test` → `/review` → `/ship` |
| Clear requirements | `/plan` → `/build` → `/test` → `/review` → `/ship` |
| Bug fix | Debug → TDD → Review → Ship |
| Refactor | Simplify → TDD → Review → Ship |
| Hotfix | Debug → Fix → Test → Ship (fast track) |
| Trivial change | TDD + Commit |

### Agent Personas

| Agent | Role | Use Case |
|-------|------|----------|
| `dev-workflow` | Orchestrator | Routes tasks through the lifecycle |
| `code-reviewer` | Senior Staff Engineer | 5-axis code review |
| `security-auditor` | Security Engineer | OWASP assessment, threat modeling |
| `test-engineer` | QA Specialist | Test strategy, coverage analysis |

## How It Works

### 1. Task Classification
Every incoming task is classified (new feature, bug fix, refactor, hotfix, etc.) and routed to the appropriate workflow path.

### 2. Phase Execution
Each phase activates the right agent-skills automatically:

| Phase | Skills Activated |
|-------|-----------------|
| Define | `interview-me`, `idea-refine`, `spec-driven-development` |
| Plan | `planning-and-task-breakdown` |
| Build | `incremental-implementation`, `test-driven-development`, `source-driven-development`, UI Builder |
| Verify | `debugging-and-error-recovery`, `browser-testing-with-devtools` |
| Review | `code-review-and-quality`, `security-and-hardening`, `performance-optimization` |
| Ship | `git-workflow-and-versioning`, `ci-cd-and-automation`, `shipping-and-launch` |

### 3. Stack-Aware UI Routing
During the Build phase, the workflow auto-detects the project stack and routes UI work to the correct Syncfusion UI Builder — no manual configuration needed.

## Package Structure

```
software-dev-workflow/
├── apm.yml                                    # Package manifest + dependencies
├── README.md                                  # This file
├── .apm/
│   ├── agents/                                # 4 specialist agents
│   │   ├── dev-workflow.agent.md              #   Workflow orchestrator
│   │   ├── code-reviewer.agent.md             #   Code review specialist
│   │   ├── security-auditor.agent.md          #   Security audit specialist
│   │   └── test-engineer.agent.md             #   QA specialist
│   ├── skills/
│   │   └── software-dev-workflow/             # Main orchestration skill
│   │       ├── SKILL.md                       #   Workflow definition
│   │       └── references/                    #   Phase-specific instructions
│   │           ├── phase-define.md
│   │           ├── phase-plan.md
│   │           ├── phase-build.md
│   │           ├── phase-verify.md
│   │           ├── phase-review.md
│   │           ├── phase-ship.md
│   │           └── workflow-routes.md
│   ├── prompts/                               # 6 slash commands
│   │   ├── spec.prompt.md                     #   /spec
│   │   ├── plan.prompt.md                     #   /plan
│   │   ├── build.prompt.md                    #   /build
│   │   ├── test.prompt.md                     #   /test
│   │   ├── review.prompt.md                   #   /review
│   │   └── ship.prompt.md                     #   /ship
│   └── instructions/
│       └── workflow-rules.instructions.md     # Always-on project rules
```

## Engineering Principles

This workflow enforces practices from [Software Engineering at Google](https://abseil.io/resources/swe-book):

- **Spec before code** — Non-trivial work without a spec is guessing
- **Vertical slices** — Build complete feature paths, not horizontal layers
- **Test-driven development** — Write the failing test first
- **~100 lines per change** — Small, reviewable increments
- **Code review before merge** — Every change, no exceptions
- **Verify at runtime** — "Seems right" is never sufficient
- **Anti-rationalization** — The agent cannot talk itself out of doing the right thing

## License

MIT
