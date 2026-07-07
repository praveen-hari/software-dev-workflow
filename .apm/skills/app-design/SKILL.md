---
name: app-design
description: >
  Design a complete application and build it directly in the target platform.
  Handles the full flow: gather requirements → plan screen architecture →
  create design system → build screens in React/Angular/Vue/Blazor using
  Syncfusion component skills. Use when: user wants to design an app, create
  an application, build a new product, design a dashboard, design an admin
  panel, design a SaaS app, or any request to design and build a full
  application UI.
---

# app-design — Design & Build Applications

Design a complete application — from idea to production code in the target platform. No HTML prototypes. Build directly in React, Angular, Vue, Blazor, or any framework.

## The Process

```
Stage 1: UNDERSTAND  →  Stage 2: PLAN  →  Stage 3: DESIGN  →  Stage 4: BUILD
(Requirements)          (Architecture)     (Design System)     (Production Code)
```

Pause after each stage for user confirmation. Do not skip stages.

If the project already has artifacts, skip completed stages:
- OpenSpec change with proposal → skip Stage 1
- `APP_ARCHITECTURE.md` exists → skip Stage 1–2
- `DESIGN.md` exists → skip Stage 1–3
- Source files exist → go to iteration

---

## Stage 1 — Understand

Gather requirements through conversation. Ask **one or two questions at a time**:

1. **What are you building?** — App type and one-line description
2. **Core features?** — 3–7 key things the app needs to do
3. **Who are the users?** — Target audience, roles, technical level
4. **What platform/framework?** — React, Angular, Vue, Blazor, etc.
5. **What's the mood/style?** — Professional, playful, minimal, bold, luxury, technical
6. **Any apps you like the look of?** — Visual references
7. **Existing brand?** — Colors, fonts, logo already decided?
8. **Dark mode needed?**

### Output

Create the OpenSpec change via `openspec-propose` with the gathered requirements. This produces `proposal.md`, `design.md`, and `tasks.md` in the change folder.

Confirm with user before proceeding.

---

## Stage 2 — Plan (Screen Architecture)

Plan every screen, navigation, and key user journeys.

### Screen Types

| Type | Description | Typical Components |
|------|-------------|-------------------|
| `dashboard` | Overview with stats | KPI cards, charts, activity feed |
| `list` | Collection with search/filter | DataGrid, filter bar, pagination |
| `detail` | Single item view | Tabs, content sections, side panel |
| `form` | Create or edit | Form inputs, validation, submit |
| `kanban` | Columns with cards | Kanban board, drag-and-drop |
| `calendar` | Date-based view | Scheduler, event details |
| `settings` | Configuration | Tabs, form sections, toggles |
| `auth` | Login/signup | Centered card, form |
| `chat` | Messaging | Conversation list, message thread |

### Navigation Patterns

| App Type | Navigation |
|----------|-----------|
| SaaS / Dashboard / Admin | Sidebar (collapsible) + Top bar |
| Mobile app | Bottom tab bar (4–5 tabs) |
| Simple tool | Minimal top bar |
| E-commerce | Top nav + category sidebar |

### Output

Create `APP_ARCHITECTURE.md` in the project with:
- Navigation pattern and items
- Screen inventory table (name, type, priority, description)
- Key user journeys (screen → screen → screen)
- Priority: P0 (core, 4–8 screens), P1 (important, 3–6), P2 (nice-to-have)

Confirm with user before proceeding.

---

## Stage 3 — Design (Design System)

Activate the `design-system` skill. It creates `DESIGN.md` + `tokens.css`.

The design system must be approved before any code is written.

---

## Stage 4 — Build (Production Code)

Build screens directly in the target framework. No HTML prototypes.

### Step 1 — Detect stack and check Syncfusion skills

Check `package.json` or `*.csproj` for the framework. If Syncfusion component skills are installed, use them. If not, suggest installation:

```
npx skills add syncfusion/<framework>-ui-components-skills -y
```

### Step 2 — Configure theming

**⚠️ MANDATORY:** After confirming the framework, consult the matching Syncfusion themes skill for detailed implementation guidance:

| Framework | Themes Skill |
|-----------|-------------|
| React | `syncfusion-react-themes` |
| Angular | `syncfusion-angular-themes` |
| Vue | `syncfusion-vue-themes` |
| Blazor | `syncfusion-blazor-themes` |
| JavaScript | `syncfusion-javascript-themes` |
| ASP.NET Core | `syncfusion-aspnetcore-themes` |
| WPF | `syncfusion-wpf-themes` |
| WinForms | `syncfusion-winforms-themes` |

The themes skill provides:
- How to integrate `tokens.css` with Syncfusion's theming system for that framework
- Correct CSS variable mapping between design tokens and Syncfusion theme variables
- Theme registration and configuration specific to the framework

Do not skip this step — incorrect theming leads to visual inconsistencies between custom UI and Syncfusion components.

### Step 3 — Map screens to components

For each screen in the architecture, map UI elements to Syncfusion components:

| Screen Element | Syncfusion Component |
|---------------|---------------------|
| Data table | DataGrid |
| Charts/graphs | Charts, AccumulationChart |
| Calendar/schedule | Scheduler |
| Kanban board | Kanban |
| Form inputs | TextBox, NumericTextBox, DropDownList |
| Date picker | DatePicker, DateTimePicker |
| Sidebar nav | Sidebar |
| Tabs | Tab |
| Modal/dialog | Dialog |
| Toast/notification | Toast |
| Rich text | RichTextEditor |
| File upload | Uploader |
| Tree navigation | TreeView |
| Dashboard layout | DashboardLayout |

### Step 4 — Build in priority order

Build P0 screens first, then P1, then P2. For each screen:

1. Create the component/page in the framework's conventions
2. Use `tokens.css` variables for all styling — no hardcoded values
3. Use Syncfusion component skills for correct API usage, imports, and setup
4. Include realistic sample data — never Lorem ipsum
5. Wire up navigation between screens

### Step 5 — Preview

Start the dev server and open in the integrated browser to verify.

---

## Iteration

| User wants | What to do |
|-----------|-----------|
| Change a color/font | Update `DESIGN.md` → re-export `tokens.css` (components auto-update via CSS variables) |
| Change a screen's content | Edit that component only |
| Add a new screen | Generate component + add route/nav item + update architecture |
| Change navigation | Update layout component + routing |
| Swap a component | Read the new Syncfusion component skill, replace in the screen |
