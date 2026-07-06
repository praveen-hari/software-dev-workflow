---
description: "Finalize the design system before building UI. Creates DESIGN.md with confirmed color palette, typography, spacing tokens, and exports tokens.css. Use before /build when the project has frontend UI work."
argument-hint: "Describe the visual style, or leave blank for guided wizard"
---

# /design — Finalize the Design System

Execute **Phase 2.5 (Design)** of the software development workflow.

## Prerequisites

This phase is required before `/build` for any project with frontend UI.
Skip only for backend-only work or if `DESIGN.md` already exists and is approved.

## Steps

1. **MUST activate the `design-system` skill** — read it completely before proceeding
2. Read `references/phase-design.md` for the full phase process
3. Follow the `design-system` skill's 4-step workflow exactly:
   - Step 1: Gather Design Preferences
   - Step 2: Choose Design Tokens
   - Step 3: Document in DESIGN.md
   - Step 4: Export tokens.css
4. If the user provided a style description, use it as input to Step 1
5. If not, run the guided wizard from the `design-system` skill:
   a. Ask about mood/style (professional, minimal, bold, playful, technical, luxury)
   b. Ask about visual references (apps they like the look of)
   c. Ask about existing brand (colors, fonts, logo)
   d. Ask about dark mode needs
5. Choose design tokens based on preferences (colors, typography, spacing, radii, components)
6. Verify WCAG contrast ratios (4.5:1 for normal text, 3:1 for large text)
7. Write `DESIGN.md` with YAML tokens + Markdown summary
8. Export `tokens.css` with CSS custom properties
9. Present the design system summary for user review — do NOT proceed until approved

## Output

- `DESIGN.md` — Design tokens (YAML front matter) + human-readable summary (project root)
- `tokens.css` — CSS custom properties for all tokens (project root)

> **Important:** `DESIGN.md` is a **project-level** file, not per-feature.
> It defines the shared design system (colors, typography, spacing) for the entire application.
> All features consume the same tokens. It lives at the project root, never inside `openspec/changes/`.

## What Gets Confirmed

| Category | What the User Approves |
|----------|----------------------|
| Colors | Primary, secondary, accent, background, surface, border, success, warning, error |
| Dark mode | Dark color variants (if requested) |
| Typography | Font family, size scale (h1–caption), weights |
| Spacing | Scale from xs to xl |
| Border radii | Sharp, subtle, or rounded |
| Components | Button, card, and input base styles |

## Rules

- No UI code is written until DESIGN.md is approved
- All UI code must use CSS variables from tokens.css — no hardcoded values
- Accent color is ONLY for interactive elements (buttons, links, focus rings)
- One font family per project unless the user explicitly requests two
