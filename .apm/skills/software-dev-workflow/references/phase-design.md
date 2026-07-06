# Phase 2.5 — Design (Finalize Design System)

## Purpose

Establish and confirm the complete visual design system — colors, typography,
spacing, border radii, and component styles — before any UI code is written.
This phase produces a `DESIGN.md` with design tokens and a `tokens.css` with
CSS custom properties that all UI code must consume.

**Core principle:** Design before code. No UI implementation starts until the
design system is confirmed by the user.

## When to Enter

- The project involves frontend UI (web, mobile, or desktop)
- No `DESIGN.md` exists at the project root
- Coming from Phase 2 (Plan) with UI tasks in the task list

## When to Skip

- Backend-only work (APIs, databases, CLI tools)
- `DESIGN.md` already exists and is approved
- Bug fixes that don't change visual design
- The user explicitly says "skip design"

## Skill Activated

### `design-system` (MUST activate)

You **MUST** activate the `design-system` skill. Do NOT create `DESIGN.md` manually or skip this skill. It runs a 4-step process:

### Step 1: Gather Design Preferences

Ask the user about mood, style references, existing brand, dark mode needs,
and platform. Use the mood → token mapping table in the skill to guide choices.

**Skip if:** The user has already provided clear design direction.

### Step 2: Choose Design Tokens

Define all required tokens based on preferences:

| Category | Minimum | What It Covers |
|----------|---------|----------------|
| Colors | 9 tokens | primary, secondary, accent, background, surface, border, success, warning, error |
| Dark colors | 9 tokens | Same keys, dark variants (if dark mode requested) |
| Typography | 6 levels | h1, h2, h3, body, small, caption |
| Spacing | 5 levels | xs, sm, md, lg, xl |
| Border radii | 4 levels | sm, md, lg, full |
| Components | 3 patterns | button, card, input |

### Step 3: Write DESIGN.md

Create `DESIGN.md` with:
- YAML front matter containing all design tokens
- Markdown body with a concise human-readable summary

**Location:** Always at the **project root** — `DESIGN.md` is a project-level design system shared by all features, not per-feature. It never goes inside `openspec/changes/`.

### Step 4: Export tokens.css

Generate `tokens.css` with CSS custom properties derived from the YAML tokens.
Save at the **project root** alongside `DESIGN.md`.

## User Gate

Present the design system summary and wait for approval:

```
DESIGN SYSTEM SUMMARY:
- Mood: [Professional / Minimal / Bold / etc.]
- Accent: [color] — used for buttons, links, focus rings
- Font: [font family] — [size range]
- Dark mode: [Yes / No]
- Border radius: [Sharp / Subtle / Rounded]

→ Approve this design direction, or tell me what to change.
```

**Do NOT proceed to Phase 3 (Build) until the user approves the design system.**

## Exit Criteria

- [ ] `DESIGN.md` created with complete YAML tokens
- [ ] All required color, typography, spacing, and component tokens defined
- [ ] WCAG contrast ratios verified (4.5:1 for normal text, 3:1 for large text)
- [ ] `tokens.css` exported with CSS custom properties
- [ ] User has reviewed and approved the design system

## Output Artifacts

- `DESIGN.md` — Design tokens (YAML) + human-readable summary (Markdown)
- `tokens.css` — CSS custom properties for all tokens

## Transition

→ Proceed to **Phase 3 — Build** (read `phase-build.md`)
→ All UI code in the Build phase MUST consume `tokens.css` — no hardcoded values
