# SDW CLI — Architecture

## Overview

`sdw` is a standalone CLI + MCP server that serves as the **context provider** for AI agents during the complete software development lifecycle. The agent is the brain; the CLI is the library.

```
┌─────────────────────────────────────────────────────────────────┐
│                        AI Agent (Brain)                         │
│                                                                 │
│  Makes decisions, writes code, runs tests, reviews, ships       │
│                                                                 │
│  Calls sdw via:                                                 │
│  ├── MCP tool calls (primary — agent invokes directly)          │
│  └── CLI subprocess (fallback — agent runs shell command)       │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                    sdw CLI / MCP Server                          │
│                    (Context Provider)                            │
│                                                                 │
│  ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌───────────┐       │
│  │  Project   │ │  Workflow  │ │   Skill   │ │  Context  │       │
│  │  Manager   │ │  Engine   │ │  Registry │ │  Builder  │       │
│  └─────┬─────┘ └─────┬─────┘ └─────┬─────┘ └─────┬─────┘       │
│        │             │             │             │               │
│  ┌─────▼─────────────▼─────────────▼─────────────▼─────┐        │
│  │                  State Store                         │        │
│  │  (reads project files, tracks phase, caches skills)  │        │
│  └──────────────────────────────────────────────────────┘        │
└─────────────────────────────────────────────────────────────────┘
```

---

## Core Modules

### 1. Project Manager

Handles project initialization and configuration. Detects the project stack, sets up all dependencies in one command.

```
sdw init
  │
  ├── Detect stack (React? Angular? Blazor? etc.)
  ├── Install agent-skills (24 engineering skills)
  ├── Install framework-specific UI Builder skills (e.g., 64 React skills)
  ├── Initialize OpenSpec (openspec/)
  ├── Scaffold DESIGN.md template (if UI project)
  ├── Configure AI agent (Copilot/Claude/Cursor/Codex)
  │   └── Write agent-specific config files (.github/copilot-instructions.md, etc.)
  ├── Create .sdw/config.json (project config)
  └── Done — one command, everything ready
```

**Config file:** `.sdw/config.json`
```json
{
  "version": "1.0.0",
  "stack": "react",
  "framework": "next",
  "uiLibrary": "syncfusion",
  "agents": ["copilot", "claude"],
  "features": {
    "openspec": true,
    "designSystem": true,
    "uiBuilder": true
  },
  "skills": {
    "installed": ["addyosmani/agent-skills", "syncfusion/react-ui-builder"],
    "registry": "https://github.com/syncfusion/react-ui-components-skills"
  }
}
```

**Commands:**
```
sdw init                          # Full project setup (interactive wizard)
sdw init --stack react --agent copilot  # Non-interactive setup
sdw config get stack              # Read config
sdw config set agent copilot      # Update config
```

---

### 2. Workflow Engine

Knows the 7-phase lifecycle, tracks current phase, validates gates, recommends next steps.

```
DEFINE ──▶ PLAN ──▶ DESIGN ──▶ BUILD ──▶ VERIFY ──▶ REVIEW ──▶ SHIP
  │          │         │         │          │          │         │
  │          │         │         │          │          │         │
  ▼          ▼         ▼         ▼          ▼          ▼         ▼
OpenSpec   tasks.md  DESIGN.md  code      tests     review    deploy
propose    refine    tokens     impl      debug     approve   archive
```

**State tracking:** Reads project files to determine current phase.

```
Phase detection logic:
  │
  ├── No openspec/changes/<name>/ exists?
  │   → Phase: DEFINE (need to run /spec)
  │
  ├── openspec/changes/<name>/proposal.md exists but no tasks.md?
  │   → Phase: PLAN (need to run /plan)
  │
  ├── tasks.md exists but no DESIGN.md (and UI tasks detected)?
  │   → Phase: DESIGN (need to run /design)
  │
  ├── DESIGN.md exists, tasks unchecked?
  │   → Phase: BUILD (ready to implement)
  │
  ├── All tasks checked, tests not verified?
  │   → Phase: VERIFY (need to run /test)
  │
  ├── Tests pass, no review approval?
  │   → Phase: REVIEW (need to run /review)
  │
  └── Review approved?
      → Phase: SHIP (ready to deploy)
```

**Gate validation:**
```
sdw validate --phase build
  │
  ├── ✓ openspec/changes/<feature>/tasks.md exists
  ├── ✓ tasks.md has unchecked items (work to do)
  ├── ✓ DESIGN.md exists at project root (UI tasks detected)
  ├── ✓ tokens.css exists at project root
  ├── ✓ Syncfusion React UI Builder skills installed
  └── Result: PASS — ready to build
```

```
sdw validate --phase build
  │
  ├── ✓ openspec/changes/<feature>/tasks.md exists
  ├── ✗ DESIGN.md missing (UI tasks detected in tasks.md)
  └── Result: FAIL — run /design first
```

**Commands:**
```
sdw status                        # Current phase, feature, progress
sdw status --feature add-dashboard  # Status for specific feature
sdw validate --phase build        # Check gates for a phase
sdw next                          # What should the agent do next?
sdw phases                        # List all phases with status
```

---

### 3. Skill Registry

The core innovation. Manages 400+ skills across 7 frameworks. Provides search, filter, install, and serve capabilities.

```
Skill Registry Architecture:

┌─────────────────────────────────────────────────────┐
│                   Skill Registry                     │
│                                                      │
│  ┌─────────────┐  ┌──────────────┐  ┌────────────┐  │
│  │  Index       │  │  Installer   │  │  Resolver  │  │
│  │             │  │              │  │            │  │
│  │ Reads all   │  │ Clones repo  │  │ Matches    │  │
│  │ SKILL.md    │  │ Copies skill │  │ query to   │  │
│  │ frontmatter │  │ Creates      │  │ best skill │  │
│  │ Builds      │  │ symlinks     │  │            │  │
│  │ search index│  │              │  │            │  │
│  └──────┬──────┘  └──────┬───────┘  └─────┬──────┘  │
│         │                │                │          │
│  ┌──────▼────────────────▼────────────────▼──────┐   │
│  │              Skill Cache                       │   │
│  │  ~/.sdw/skills/                                │   │
│  │  ├── index.json (search index)                 │   │
│  │  ├── syncfusion-react-grid/                    │   │
│  │  │   ├── SKILL.md                              │   │
│  │  │   └── references/                           │   │
│  │  ├── syncfusion-react-charts/                  │   │
│  │  └── ... (installed on demand)                 │   │
│  └────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────┘
```

**Index structure:** `~/.sdw/skills/index.json`
```json
{
  "version": "1.0.0",
  "lastUpdated": "2026-07-07T10:00:00Z",
  "frameworks": {
    "react": {
      "repo": "syncfusion/react-ui-components-skills",
      "skillCount": 64,
      "skills": [
        {
          "name": "syncfusion-react-grid",
          "description": "DataGrid with sorting, filtering, grouping, editing, export",
          "keywords": ["grid", "table", "datagrid", "data-table", "sorting", "filtering"],
          "category": "Data",
          "installed": true,
          "path": "~/.sdw/skills/syncfusion-react-grid"
        },
        {
          "name": "syncfusion-react-charts",
          "description": "30+ chart types for data visualization",
          "keywords": ["chart", "graph", "visualization", "line", "bar", "pie"],
          "category": "Charts",
          "installed": false
        }
      ]
    },
    "angular": { "repo": "syncfusion/angular-ui-components-skills", "skillCount": 62, "skills": [] },
    "blazor": { "repo": "syncfusion/blazor-ui-components-skills", "skillCount": 65, "skills": [] }
  },
  "workflow": {
    "repo": "addyosmani/agent-skills",
    "skillCount": 24,
    "skills": [
      { "name": "test-driven-development", "phase": "build", "installed": true },
      { "name": "code-review-and-quality", "phase": "review", "installed": true }
    ]
  }
}
```

**Search algorithm:**
```
Agent asks: "I need a sortable data table for React"

sdw skills search "sortable data table" --stack react
  │
  ├── Tokenize query: ["sortable", "data", "table"]
  ├── Match against index:
  │   ├── syncfusion-react-grid: keywords match ["table", "datagrid", "data-table", "sorting"] → score: 0.92
  │   ├── syncfusion-react-treegrid: keywords match ["table", "tree", "sorting"] → score: 0.65
  │   └── syncfusion-react-pivot-table: keywords match ["table", "pivot"] → score: 0.40
  ├── Return top match: syncfusion-react-grid
  ├── Is it installed? Yes → return skill content
  └── Not installed? → auto-install, then return skill content
```

**Commands:**
```
sdw skills search "datagrid" --stack react    # Find skill by query
sdw skills search "chart" --stack react       # Find chart skills
sdw skills install syncfusion-react-grid      # Install specific skill
sdw skills install --all --stack react        # Install all 64 React skills
sdw skills list --stack react                 # List installed React skills
sdw skills list --stack react --available     # List all available (installed + not)
sdw skills get syncfusion-react-grid          # Return full skill content (SKILL.md)
sdw skills remove syncfusion-react-grid       # Remove a skill
sdw skills update --stack react               # Update all React skills to latest
sdw skills index --rebuild                    # Rebuild search index
```

---

### 4. Context Builder

The most important module. Generates a single, curated context document for the agent — containing only what it needs for the current task.

```
sdw context --phase build --task "Add DataGrid to dashboard" --feature add-dashboard
  │
  ├── Read workflow rules (trimmed to Build-phase rules only)
  ├── Read current task from openspec/changes/add-dashboard/tasks.md
  ├── Read DESIGN.md token summary (variable names only, not full YAML)
  ├── Resolve skill: "DataGrid" → syncfusion-react-grid
  ├── Read skill content (SKILL.md + relevant references)
  ├── Read phase instructions (phase-build.md, trimmed)
  ├── Check gates (DESIGN.md exists? UI Builder installed?)
  │
  └── Output: single markdown blob (~3K tokens)
```

**Output format:**
```markdown
# Context for: Build Phase — Task 2.1: Add DataGrid to Dashboard

## Current State
- Feature: add-dashboard
- Phase: BUILD
- Task: 2.1 — Add DataGrid component to dashboard page
- Stack: React (Next.js)
- Design System: ✓ DESIGN.md approved, tokens.css available
- UI Builder: ✓ syncfusion-react-ui-builder installed

## Workflow Rules (Build Phase)
- Implement the smallest complete piece of functionality
- Write failing test first (TDD), then make it pass
- All UI code MUST use CSS variables from tokens.css
- Commit after each verified slice

## Design Tokens Available
--color-primary, --color-secondary, --color-accent, --color-background,
--color-surface, --color-border, --color-success, --color-warning, --color-error
--font-h1, --font-h2, --font-h3, --font-body, --font-small, --font-caption
--spacing-xs, --spacing-sm, --spacing-md, --spacing-lg, --spacing-xl
--rounded-sm, --rounded-md, --rounded-lg, --rounded-full

## Active Skill: syncfusion-react-grid
[Full SKILL.md content for the DataGrid component]

## Acceptance Criteria (from tasks.md)
- [ ] DataGrid renders with sample data
- [ ] Sorting enabled on all columns
- [ ] Filtering enabled with filter bar
- [ ] Pagination with 20 rows per page
- [ ] Uses design tokens for all styling
```

**Commands:**
```
sdw context --phase build --task "2.1"        # Context for specific task
sdw context --phase build                     # Context for current build phase
sdw context --phase design                    # Context for design phase
sdw context --phase review                    # Context for review (includes design compliance)
sdw context --raw                             # Full unfiltered context (all files)
```

---

## Dual Interface: CLI + MCP Server

### CLI Mode (Developer-facing)

```bash
# Setup
sdw init
sdw init --stack react --agent copilot

# Status
sdw status
sdw next
sdw validate --phase build

# Skills
sdw skills search "datagrid" --stack react
sdw skills install syncfusion-react-grid
sdw skills list

# Context
sdw context --phase build --task "2.1"
```

### MCP Server Mode (Agent-facing)

The same functionality exposed as MCP tools that any AI agent can call:

```json
{
  "tools": [
    {
      "name": "sdw_status",
      "description": "Get current workflow status — phase, feature, progress, next step",
      "parameters": {
        "feature": { "type": "string", "description": "Feature name (optional)" }
      }
    },
    {
      "name": "sdw_validate",
      "description": "Check if prerequisites are met for a phase",
      "parameters": {
        "phase": { "type": "string", "enum": ["define", "plan", "design", "build", "verify", "review", "ship"] }
      }
    },
    {
      "name": "sdw_context",
      "description": "Get curated context for the current phase and task",
      "parameters": {
        "phase": { "type": "string" },
        "task": { "type": "string", "description": "Task ID or description" },
        "feature": { "type": "string" }
      }
    },
    {
      "name": "sdw_skills_search",
      "description": "Find the right Syncfusion component skill for a task",
      "parameters": {
        "query": { "type": "string", "description": "What component do you need?" },
        "stack": { "type": "string", "description": "Framework (react, angular, blazor, etc.)" }
      }
    },
    {
      "name": "sdw_skills_get",
      "description": "Get the full content of a specific skill",
      "parameters": {
        "name": { "type": "string", "description": "Skill name (e.g., syncfusion-react-grid)" }
      }
    },
    {
      "name": "sdw_skills_install",
      "description": "Install a skill that is not yet available locally",
      "parameters": {
        "name": { "type": "string" }
      }
    },
    {
      "name": "sdw_next",
      "description": "Get the recommended next action based on current project state",
      "parameters": {}
    }
  ]
}
```

**MCP startup:**
```bash
# Start as MCP server (stdio transport)
sdw serve

# Or configure in agent's MCP config:
# .vscode/mcp.json / claude_desktop_config.json / etc.
{
  "mcpServers": {
    "sdw": {
      "command": "sdw",
      "args": ["serve"],
      "transport": "stdio"
    }
  }
}
```

---

## Directory Structure

### CLI Package
```
sdw/
├── package.json                    # @syncfusion/sdw
├── bin/
│   └── sdw.js                      # CLI entry point
├── src/
│   ├── cli.ts                      # Commander.js CLI definition
│   ├── mcp-server.ts               # MCP server (stdio transport)
│   │
│   ├── modules/
│   │   ├── project-manager.ts      # sdw init, config
│   │   ├── workflow-engine.ts       # phase detection, gate validation, next step
│   │   ├── skill-registry.ts        # search, install, list, get, index
│   │   └── context-builder.ts       # curated context generation
│   │
│   ├── state/
│   │   ├── project-state.ts         # Read project files, detect stack
│   │   ├── phase-detector.ts        # Determine current phase from file state
│   │   └── task-tracker.ts          # Read tasks.md, track checked/unchecked
│   │
│   ├── skills/
│   │   ├── index-builder.ts         # Build search index from SKILL.md frontmatter
│   │   ├── search-engine.ts         # Keyword + fuzzy matching
│   │   ├── installer.ts             # Clone repo, copy skill, create symlinks
│   │   └── resolver.ts              # Query → best matching skill
│   │
│   ├── context/
│   │   ├── templates/               # Context templates per phase
│   │   │   ├── build.hbs
│   │   │   ├── design.hbs
│   │   │   ├── review.hbs
│   │   │   └── ...
│   │   ├── token-summarizer.ts      # Extract CSS variable names from DESIGN.md
│   │   └── rule-trimmer.ts          # Extract phase-relevant rules only
│   │
│   └── utils/
│       ├── git.ts                   # Git operations (clone, pull)
│       ├── agent-config.ts          # Write agent-specific config files
│       └── logger.ts                # Structured logging
│
├── data/
│   └── skill-repos.json             # Registry of all Syncfusion skill repos
│
└── tests/
    ├── workflow-engine.test.ts
    ├── skill-registry.test.ts
    ├── context-builder.test.ts
    └── ...
```

### Project-level files (created by `sdw init`)
```
my-project/
├── .sdw/
│   ├── config.json                  # Project config (stack, agents, features)
│   └── cache/
│       └── skill-index.json         # Local skill search index
├── DESIGN.md                        # Design system (project-level)
├── tokens.css                       # CSS variables from DESIGN.md
├── openspec/                        # OpenSpec artifacts
│   ├── specs/                       # Source of truth
│   └── changes/                     # Active features
└── .github/copilot-instructions.md  # Agent config (auto-generated)
```

---

## Data Flow: Agent Calls CLI

### Example: Agent building a dashboard

```
1. Agent receives: "Build a dashboard with charts and a data table"

2. Agent calls: sdw_status()
   CLI returns: { phase: "build", feature: "add-dashboard", task: "2.1" }

3. Agent calls: sdw_validate({ phase: "build" })
   CLI returns: { pass: true, design: true, uiBuilder: true }

4. Agent calls: sdw_skills_search({ query: "data table sortable", stack: "react" })
   CLI returns: { match: "syncfusion-react-grid", installed: true, confidence: 0.92 }

5. Agent calls: sdw_context({ phase: "build", task: "2.1", feature: "add-dashboard" })
   CLI returns: curated markdown with:
     - Build phase rules
     - Design tokens summary
     - syncfusion-react-grid SKILL.md content
     - Task acceptance criteria

6. Agent writes code using the skill instructions and design tokens

7. Agent calls: sdw_skills_search({ query: "line chart area chart", stack: "react" })
   CLI returns: { match: "syncfusion-react-charts", installed: false }

8. Agent calls: sdw_skills_install({ name: "syncfusion-react-charts" })
   CLI installs the skill and returns: { success: true, path: "~/.sdw/skills/syncfusion-react-charts" }

9. Agent calls: sdw_skills_get({ name: "syncfusion-react-charts" })
   CLI returns: full SKILL.md content for charts

10. Agent continues building with chart skill instructions
```

---

## Technology Stack

| Component | Technology | Why |
|---|---|---|
| Language | TypeScript | Type safety, npm ecosystem, same as target projects |
| CLI framework | Commander.js | Mature, well-documented, supports subcommands |
| MCP server | @modelcontextprotocol/sdk | Official MCP SDK for tool exposure |
| Search | Fuse.js | Lightweight fuzzy search for skill matching |
| Git operations | simple-git | Programmatic git clone/pull for skill installation |
| File system | fs-extra | Cross-platform file operations |
| Config | cosmiconfig | Standard config file discovery (.sdw/config.json) |
| Templates | Handlebars | Context template rendering |
| Testing | Vitest | Fast, TypeScript-native testing |
| Build | tsup | Fast TypeScript bundler for CLI |

---

## Token Budget Analysis

| Scenario | Without sdw | With sdw |
|---|---|---|
| Agent figures out phase | ~3,200 tokens (reads SKILL.md) | ~50 tokens (calls sdw_status) |
| Agent finds right skill | ~6,000 tokens (scans 64 skill descriptions) | ~100 tokens (calls sdw_skills_search) |
| Agent gets build context | ~9,000 tokens (reads 5-6 files) | ~3,000 tokens (calls sdw_context) |
| Agent validates gates | ~1,100 tokens (reads workflow-rules) | ~50 tokens (calls sdw_validate) |
| **Total per task** | **~19,300 tokens** | **~3,200 tokens** |
| **Savings** | — | **~83% reduction** |
