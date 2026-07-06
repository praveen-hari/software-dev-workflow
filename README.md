# Software Development Workflow

Production-grade software development workflow for AI coding agents. Orchestrates a 6-phase lifecycle using [agent-skills](https://github.com/addyosmani/agent-skills) for engineering discipline and [Syncfusion UI Builders](https://www.syncfusion.com/explore/agentic-ui-builder/) for frontend generation across 7 frameworks.

```
  DEFINE       PLAN        DESIGN        BUILD        VERIFY       REVIEW        SHIP
 ┌────────┐  ┌────────┐  ┌────────┐  ┌──────┐  ┌──────┐  ┌──────┐  ┌──────┐
 │ Explore│─▶│  Tasks │─▶│ Colors │─▶│ Code │─▶│ Test │─▶│  QA  │─▶│  Go  │
 │Propose │  │  Plan  │  │ Tokens │  │ Impl │  │Debug │  │ Gate │  │ Live │
 └────────┘  └────────┘  └────────┘  └──────┘  └──────┘  └──────┘  └──────┘
  /spec       /plan       /design       /build      /test      /review     /ship
```

## What This Package Does

- **Orchestrates 24 agent-skills** through a structured 6-phase lifecycle
- **Auto-detects project stack** and routes UI work to the correct Syncfusion UI Builder (installed per-project)
- **Enforces engineering discipline**: spec before code, TDD, code review, security hardening
- **Provides 7 slash commands** (`/spec`, `/plan`, `/design`, `/build`, `/test`, `/review`, `/ship`)
- **Includes 4 specialist agents**: code reviewer, security auditor, test engineer, workflow orchestrator
- **Works for any tech stack** — the engineering workflow is stack-agnostic

## Supported Frameworks (UI Generation)

UI Builders are installed **per-project** (not bundled) to keep context lean. Each builder adds ~60–70 component skills. Install only the one matching your project's framework.

| Framework | Install Command | Stack |
|-----------|----------------|-------|
| React | `apm install syncfusion/react-ui-builder -t <target>` | Web |
| Angular | `apm install syncfusion/angular-ui-builder -t <target>` | Web |
| Blazor | `apm install syncfusion/blazor-ui-builder -t <target>` | Web (.NET) |
| .NET MAUI | `apm install syncfusion/maui-ui-builder -t <target>` | Cross-platform |
| WPF | `apm install syncfusion/wpf-ui-builder -t <target>` | Windows Desktop |
| WinForms | `apm install syncfusion/winforms-ui-builder -t <target>` | Windows Desktop |
| WinUI | `apm install syncfusion/winui-ui-builder -t <target>` | Windows Desktop |

> **Why not bundle all 7?** Each UI Builder pulls ~60–70 component skills as
> transitive dependencies. Bundling all 7 would install ~500+ skills, flooding
> the agent's context window with irrelevant framework knowledge. A React project
> never needs WPF skills.

For non-Syncfusion stacks, the workflow uses `agent-skills` directly (incremental-implementation + frontend-ui-engineering).

## Prerequisites

- [APM (Agent Package Manager)](https://microsoft.github.io/apm/getting-started/installation/#quick-install-recommended)
- [OpenSpec](https://openspec.dev/) — spec-driven planning layer for Define + Plan phases
- A supported AI agent or IDE (Code Studio, VS Code, Cursor, Claude Code, etc.)
- Node.js 20.19.0+ (required by OpenSpec)

## Installation

### Step 1 — Install the base workflow

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

This installs:
- ✅ 24 agent-skills (engineering workflows)
- ✅ 4 specialist agents + 6 slash commands
- ✅ Workflow orchestration skill with phase references

### Step 2 — Initialize OpenSpec (for Define + Plan phases)

```bash
npm install -g @fission-ai/openspec@latest
cd your-project
openspec init
```

This sets up the `openspec/` directory structure and installs slash commands (`/opsx:explore`, `/opsx:propose`, `/opsx:apply`, `/opsx:archive`) into your AI tool.

> **Why OpenSpec?** It provides per-feature artifact folders (`openspec/changes/<name>/`)
> with co-located proposal, specs, design, and tasks. No more overwriting shared
> `tasks/todo.md` files when working on multiple features.

### Step 3 — Install your framework's UI Builder (optional)

If your project uses Syncfusion components, install **only** the builder for your framework:

```bash
# Example: React project
apm install syncfusion/react-ui-builder -t copilot

# Example: Blazor project
apm install syncfusion/blazor-ui-builder -t copilot
```

See the [Supported Frameworks](#supported-frameworks-ui-generation) table for all options.

> **Tip:** If you skip this step, the workflow will auto-detect your stack during
> `/build` and prompt you with the correct install command.

## Usage

### Slash Commands

| Command | Phase | What Happens |
|---------|-------|-------------|
| `/spec` | Define | Uses OpenSpec: `/opsx:explore` → `/opsx:propose` → creates change folder with proposal, specs, design, tasks |
| `/plan` | Plan | Refines `openspec/changes/<name>/tasks.md` with vertical slices and acceptance criteria |
| `/design` | Design | Finalizes DESIGN.md with color palette, typography, spacing tokens + exports tokens.css. Required before UI code. |
| `/build` | Build | Reads tasks from OpenSpec change folder, implements slice-by-slice with TDD + UI Builder routing. UI code consumes tokens.css. |
| `/test` | Verify | Run tests, debug failures, verify at runtime |
| `/review` | Review | 5-axis code review + security audit + test coverage |
| `/ship` | Ship | Git workflow → CI/CD → Deploy → Monitor → then `/opsx:archive` to merge specs |

### Workflow Routes

Not every task needs the full lifecycle:

| Task Type | Route |
|-----------|-------|
| New feature (with UI) | `/spec` → `/plan` → `/design` → `/build` → `/test` → `/review` → `/ship` → `/opsx:archive` |
| New feature (backend only) | `/spec` → `/plan` → `/build` → `/test` → `/review` → `/ship` → `/opsx:archive` |
| Clear requirements | `/opsx:propose` → `/plan` → `/design` → `/build` → `/test` → `/review` → `/ship` → `/opsx:archive` |
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
| Define | OpenSpec `/opsx:explore`, `/opsx:propose` |
| Plan | `planning-and-task-breakdown` |
| Design | `design-system` — colors, typography, spacing, component tokens |
| Build | `incremental-implementation`, `test-driven-development`, `source-driven-development`, UI Builder |
| Verify | `debugging-and-error-recovery`, `browser-testing-with-devtools` |
| Review | `code-review-and-quality`, `security-and-hardening`, `performance-optimization` |
| Ship | `git-workflow-and-versioning`, `ci-cd-and-automation`, `shipping-and-launch` |

### 3. Stack-Aware UI Routing
During the Build phase, the workflow auto-detects the project stack and checks if the matching Syncfusion UI Builder is installed. If installed, it routes UI work to the builder. If not, it prompts you with the install command and falls back to `agent-skills` for UI work.

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
│   │   ├── design-system/                     # Design system skill
│   │   │   └── SKILL.md                       #   Color, typography, spacing tokens
│   │   └── software-dev-workflow/             # Main orchestration skill
│   │       ├── SKILL.md                       #   Workflow definition
│   │       └── references/                    #   Phase-specific instructions
│   │           ├── phase-define.md
│   │           ├── phase-plan.md
│   │           ├── phase-design.md
│   │           ├── phase-build.md
│   │           ├── phase-verify.md
│   │           ├── phase-review.md
│   │           ├── phase-ship.md
│   │           └── workflow-routes.md
│   ├── prompts/                               # 7 slash commands
│   │   ├── spec.prompt.md                     #   /spec
│   │   ├── plan.prompt.md                     #   /plan
│   │   ├── design.prompt.md                   #   /design
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
- **Design before UI code** — Color system, typography, and spacing must be confirmed before implementation
- **Vertical slices** — Build complete feature paths, not horizontal layers
- **Test-driven development** — Write the failing test first
- **~100 lines per change** — Small, reviewable increments
- **Code review before merge** — Every change, no exceptions
- **Verify at runtime** — "Seems right" is never sufficient
- **Anti-rationalization** — The agent cannot talk itself out of doing the right thing

## License

MIT
