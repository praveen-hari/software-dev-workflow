# Software Development Workflow

Lightweight development workflow for AI coding agents. Uses [OpenSpec](https://openspec.dev/) as the orchestrator with built-in engineering discipline gates.

```
  THINK              PLAN               BUILD + SHIP
 ┌──────────┐      ┌──────────┐      ┌──────────────────────┐
 │ /explore │─────▶│ /propose │─────▶│ /apply               │
 │          │      │          │      │  + engineering skills │
 └──────────┘      └──────────┘      └──────┬───────────────┘
                                            │
                                     ┌──────▼───────────┐
                                     │ /sync + /archive  │
                                     └──────────────────┘
```

## What This Does

- **OpenSpec handles the lifecycle** — explore, propose, apply, sync, archive
- **This package adds engineering discipline** — TDD, incremental delivery, code review, atomic commits
- **Auto-detects project stack** and routes UI work to Syncfusion component skills (installed per-project)
- **Works for any tech stack** — the engineering rules are stack-agnostic

## Prerequisites

- [APM](https://microsoft.github.io/apm/getting-started/installation/#quick-install-recommended)
- [OpenSpec](https://openspec.dev/) — `npm install -g @fission-ai/openspec@latest`
- A supported AI agent or IDE
- Node.js 20.19.0+

## Installation

```bash
# Install the workflow (pick your target)
apm install your-org/software-dev-workflow -t copilot

# Initialize OpenSpec in your project
cd your-project
openspec init --tools github-copilot
```

This installs:
- ✅ Workflow skill with 7 enforcement gates (spec, design system, Syncfusion skills, TDD, incremental slices, code review, atomic commits)
- ✅ App design skill — full app design flow (understand → plan → design → build directly in target platform)
- ✅ Design system skill for color palette, typography, and spacing tokens
- ✅ Always-on rules (TDD, review before merge, incremental delivery)

### Optional: Syncfusion Component Skills

If your project uses Syncfusion components, install the skills for your framework:

```bash
npx skills add syncfusion/react-ui-components-skills -y
```

| Framework | Install Command |
|-----------|----------------|
| React | `npx skills add syncfusion/react-ui-components-skills -y` |
| Angular | `npx skills add syncfusion/angular-ui-components-skills -y` |
| Vue | `npx skills add syncfusion/vue-ui-components-skills -y` |
| Blazor | `npx skills add syncfusion/blazor-ui-components-skills -y` |
| WPF | `npx skills add syncfusion/wpf-ui-components-skills -y` |
| WinForms | `npx skills add syncfusion/winforms-ui-components-skills -y` |
| .NET MAUI | `npx skills add syncfusion/maui-ui-components-skills -y` |
| ASP.NET Core | `npx skills add syncfusion/aspnetcore-ui-components-skills -y` |
| JavaScript | `npx skills add syncfusion/javascript-ui-controls-skills -y` |

> Install only the one matching your stack. Each adds ~60 component skills. The agent uses them directly.

## Usage

OpenSpec commands are your entry points:

| What you want | Command |
|---------------|---------|
| Think through an idea | `/opsx:explore` |
| Plan a change (creates proposal + specs + tasks) | `/opsx:propose` |
| Implement tasks | `/opsx:apply` |
| Sync specs back to main | `/opsx:sync` |
| Archive a completed change | `/opsx:archive` |

Not every task needs the full flow:

| Task Type | Route |
|-----------|-------|
| New feature | `explore` → `propose` → `apply` → `sync` → `archive` |
| Clear requirements | `propose` → `apply` → `sync` → `archive` |
| Bug fix | Debug → TDD → Review → Commit |
| Trivial change | Just do it |

## What Gets Enforced

During `/opsx:apply`, the workflow enforces:

- **Incremental delivery** — vertical slices, not horizontal layers
- **Test-driven development** — failing test first, then make it pass
- **Code review** — before merge, always
- **Atomic commits** — clean git history

For UI work:
- `design-system` skill creates `DESIGN.md` + `tokens.css` before any UI code
- Syncfusion component skills are used directly when installed (correct APIs, imports, setup)
- All UI code uses CSS variables from `tokens.css`

## Package Structure

```
software-dev-workflow/
├── apm.yml                                    # Package manifest + dependencies
├── README.md                                  # This file
└── .apm/
    ├── skills/
    │   ├── app-design/                        # Full app design → production code
    │   │   └── SKILL.md
    │   ├── design-system/                     # Design tokens skill
    │   │   ├── SKILL.md
    │   │   └── references/
    │   │       └── design-md-spec.md           # DESIGN.md format spec
    │   └── software-dev-workflow/             # OpenSpec router + engineering gates
    │       └── SKILL.md
    └── instructions/
        └── workflow-rules.instructions.md     # Always-on rules
```

## License

MIT
