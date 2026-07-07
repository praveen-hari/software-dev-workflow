---
name: design-system
description: >
  Finalize the visual design system before writing UI code. Creates a DESIGN.md
  with confirmed color palette, typography scale, spacing tokens, border radii,
  and component styles — exported as CSS custom properties. Use when the project
  involves frontend UI and the design system has not been established yet.
  Activates between Plan and Build phases to ensure all visual decisions are
  locked before implementation begins.
---

# Design System Skill

## Purpose

Establish and confirm the complete visual identity — colors, typography, spacing,
border radii, and component styles — before any UI code is written. The output is
a `DESIGN.md` file with YAML design tokens and a `tokens.css` file with CSS
custom properties that all UI code must consume.

**Core principle:** No hardcoded colors, fonts, or spacing in UI code. Everything
references design tokens via CSS variables.

## When to Activate

- The project involves frontend UI (web, mobile, or desktop)
- No `DESIGN.md` exists in the project yet
- The user is starting a new feature with UI components
- The user wants to change the visual direction of an existing project

## When to Skip

- Backend-only work (APIs, databases, CLI tools)
- `DESIGN.md` already exists and is approved
- Bug fixes that don't change visual design
- The user explicitly says "skip design"

## The Design Process

```
Step 1: GATHER    →  Step 2: CHOOSE    →  Step 3: DOCUMENT  →  Step 4: EXPORT
(Preferences)        (Tokens)              (DESIGN.md)          (tokens.css)
```

Pause after Step 3 for user confirmation before exporting.

---

## Step 1 — Gather Design Preferences

Ask the user (one or two questions at a time, not all at once):

1. **What's the mood/style?** — Professional, minimal, bold, playful, technical, luxury
2. **Any apps you like the look of?** — Visual references for inspiration
3. **Any existing brand?** — Colors, fonts, or logo already decided?
4. **Dark mode needed?** — Light only, dark only, or both?
5. **What platform?** — Web app, mobile app, desktop app, or responsive?

**Skip if:** The user has already provided clear design direction or brand guidelines.

### Mood → Token Mapping

| Mood | Colors | Fonts | Spacing | Radii | Shadows |
|------|--------|-------|---------|-------|---------|
| Professional | Blue/grey, muted | Inter, Source Sans | Comfortable (8px base) | Subtle (6px) | Subtle |
| Minimal | Near-black + white, one accent | Inter, Helvetica | Spacious (8px base) | Small (4px) | None/subtle |
| Bold | High-contrast, vibrant accent | Sora, Poppins | Spacious (8px base) | Rounded (12px) | Medium |
| Playful | Warm, colorful | Nunito, Poppins | Comfortable | Rounded (12px+) | Medium |
| Technical | Dark bg, code-like | JetBrains Mono, Inter | Compact (4px base) | Sharp (2px) | None |
| Luxury | Dark + gold/cream | Playfair, serif | Spacious | Subtle (4px) | Subtle |

---

## Step 2 — Choose Design Tokens

Based on the gathered preferences, define all tokens:

### Required Token Categories

| Category | Minimum Tokens | Description |
|----------|---------------|-------------|
| `colors` | 9 | primary, secondary, accent, background, surface, border, success, warning, error |
| `colors-dark` | 9 (if dark mode) | Same keys as colors, dark variants |
| `typography` | 6 | h1, h2, h3, body, small, caption |
| `spacing` | 5 | xs, sm, md, lg, xl |
| `rounded` | 4 | sm, md, lg, full |
| `components` | 3 | button, card, input |

### Color Selection Rules

- **Primary** — Main text color, headings, UI anchors
- **Secondary** — Supporting text, labels, metadata
- **Accent** — Interactive elements, CTAs, links, focus rings (ONLY for interactive elements)
- **Background** — Page background
- **Surface** — Cards, panels, elevated containers
- **Border** — Dividers, input borders, table lines
- **Success/Warning/Error** — Semantic feedback states

### WCAG Contrast Requirements

- Normal text on background: minimum 4.5:1 contrast ratio
- Large text (18px+ or 14px+ bold): minimum 3:1
- Interactive elements: minimum 3:1 against adjacent colors

---

## Step 3 — Document in DESIGN.md

Write `DESIGN.md` at the **project root** — it is a project-level design system shared by all features, not per-feature.

### DESIGN.md Format

Read [references/design-md-spec.md](references/design-md-spec.md) for the full DESIGN.md format specification (based on the [Stitch DESIGN.md spec](https://github.com/google-labs-code/design.md)).

The file has two parts: **YAML front matter** (machine-readable tokens) + **Markdown body** (human-readable summary).

### Markdown Body (keep concise)

```markdown
# [Project Name] Design System

## Overview
[1-2 sentences: mood, font, accent color]

## Colors
[1-2 sentences: accent role, semantic colors]

## Typography
[1 sentence: font name, size range]

## Spacing & Layout
[1 sentence: base unit, scale]

## Components
[1-2 sentences: button, card, input patterns]
```

### User Gate

Present the design system summary to the user:

```
DESIGN SYSTEM SUMMARY:
- Mood: [Professional / Minimal / Bold / etc.]
- Accent: [color] — used for buttons, links, focus rings
- Font: [font family] — [size range]
- Dark mode: [Yes / No]
- Border radius: [Sharp / Subtle / Rounded]

→ Approve this design direction, or tell me what to change.
```

**Do NOT proceed to Step 4 until the user approves.**

---

## Step 4 — Export tokens.css

Generate a `tokens.css` file with CSS custom properties from the YAML tokens:

```css
:root {
  /* Colors */
  --color-primary: #1A1C1E;
  --color-secondary: #6C7278;
  --color-accent: #2563EB;
  --color-background: #FFFFFF;
  --color-surface: #F8FAFC;
  --color-border: #E2E8F0;
  --color-success: #16A34A;
  --color-warning: #EAB308;
  --color-error: #DC2626;

  /* Typography */
  --font-family: 'Inter', sans-serif;
  --font-size-h1: 48px;
  --font-size-h2: 36px;
  --font-size-h3: 24px;
  --font-size-body: 16px;
  --font-size-small: 14px;
  --font-size-caption: 12px;

  /* Spacing */
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 16px;
  --space-lg: 24px;
  --space-xl: 32px;

  /* Border Radius */
  --rounded-sm: 4px;
  --rounded-md: 8px;
  --rounded-lg: 12px;
  --rounded-full: 9999px;
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
  :root {
    --color-primary: #E8EAED;
    --color-secondary: #9AA0A6;
    --color-accent: #60A5FA;
    --color-background: #121212;
    --color-surface: #1E1E1E;
    --color-border: #333333;
    --color-success: #4ADE80;
    --color-warning: #FACC15;
    --color-error: #F87171;
  }
}
```

Save `tokens.css` at the **project root** alongside `DESIGN.md`.

---

## Rules

1. **Never hardcode colors, fonts, or spacing in UI code** — always use CSS variables from tokens.css
2. **Accent color is ONLY for interactive elements** — buttons, links, focus rings, toggles
3. **Verify WCAG contrast** — primary on background ≥ 4.5:1, accent on background ≥ 4.5:1
4. **One font family per project** (unless the user explicitly requests a second for headings)
5. **Explain every choice** — tell the user WHY you picked a color, font, or spacing scale
6. **Preserve existing tokens** — when editing DESIGN.md, only change what was requested

## Exit Criteria

- [ ] `DESIGN.md` created with complete YAML tokens
- [ ] All 9 color tokens defined (+ dark mode if requested)
- [ ] Typography scale covers h1–caption (6 levels minimum)
- [ ] Spacing scale covers xs–xl (5 levels minimum)
- [ ] WCAG contrast ratios verified for text and interactive elements
- [ ] `tokens.css` exported with CSS custom properties
- [ ] User has reviewed and approved the design system
