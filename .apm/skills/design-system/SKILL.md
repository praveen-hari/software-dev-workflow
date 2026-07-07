---
name: design-system
description: >
  Create or edit a design system (DESIGN.md). Use when: user wants to change
  colors, change fonts, adjust spacing, add dark mode, update typography, add
  tokens, remove tokens, tweak design values, create a design system from a
  screenshot, extract tokens from CSS or HTML, extract design from a URL, or
  modify any design system value. Also use when user provides a screenshot,
  mockup, image, CSS file, or brand description to create a design system from.
---

# design-system — Create or Edit Design System

Create a new DESIGN.md from any source, or edit an existing one. This skill is for **targeted design system tasks** — not full app design (use the `app-design` skill for that).

## When to Use

**Creating a new design system:**
- User provides a screenshot, mockup, or design image
- User provides a CSS or HTML file to extract tokens from
- User provides a website URL to analyze
- User asks to create a design system from a text description
- The project involves frontend UI and no `DESIGN.md` exists

**Editing an existing design system:**
- User asks to change a color, font, spacing, or component token
- User asks to add or remove tokens (e.g., "add dark mode")
- User asks to update the prose/rationale sections

**Skip when:**
- Backend-only work (APIs, databases, CLI tools)
- `DESIGN.md` already exists and is approved
- Bug fixes that don't change visual design

---

## Create Flow

### Step 1 — Gather context

Ask the user about the style direction (one or two questions at a time):

1. **What's the mood/style?** — Professional, minimal, bold, playful, technical, luxury
2. **Any apps you like the look of?** — Visual references
3. **Existing brand?** — Colors, fonts, logo already decided?
4. **Dark mode needed?**

Skip if the user already provided clear direction or a source to extract from.

### Step 2 — Analyze the source

**From an image/screenshot:**
- Identify colors (primary, secondary, accent, background, surface, border)
- Identify typography (font families, sizes, weights)
- Identify spacing and border radius patterns
- Identify the overall mood

**From CSS/HTML:**
- Extract CSS custom properties and color values
- Extract font declarations and spacing values

**From a URL:**
- Fetch and analyze the rendered styles

**From a text description:**
- Interpret the mood and choose appropriate tokens
- Reference well-known design systems when mentioned (e.g., "like Linear" → clean, minimal)

#### Mood → Token Mapping

| Mood | Colors | Fonts | Radii |
|------|--------|-------|-------|
| Professional | Blue/grey, muted | Inter, Source Sans | Subtle (6px) |
| Minimal | Near-black + white, one accent | Inter, Helvetica | Small (4px) |
| Bold | High-contrast, vibrant accent | Sora, Poppins | Rounded (12px) |
| Playful | Warm, colorful | Nunito, Poppins | Rounded (12px+) |
| Technical | Dark bg, code-like | JetBrains Mono, Inter | Sharp (2px) |
| Luxury | Dark + gold/cream | Playfair, serif | Subtle (4px) |

### Step 3 — Generate DESIGN.md

Read [references/design-md-spec.md](references/design-md-spec.md) for the full format specification.

Write `DESIGN.md` at the **project root**.

**Required YAML tokens:**
- `colors` — minimum 9: primary, secondary, accent, background, surface, border, success, warning, error
- `colors-dark` — if dark mode needed, match all light color token names
- `typography` — minimum 6: h1, h2, h3, body, small, caption (each needs fontFamily + fontSize)
- `spacing` — minimum 5: xs, sm, md, lg, xl
- `rounded` — minimum 4: sm, md, lg, full
- `components` — minimum 3: button, card, input (use `{token.references}`)

**Markdown body — keep concise:**
Include at least 5 `##` section headings. Each section needs only 1–3 lines. The YAML tokens are the source of truth — the prose is just a brief summary.
- Overview (1–2 sentences: mood, font, accent, use case)
- Colors (1–2 sentences: accent role, semantic colors)
- Typography (1 sentence: font name, size range)
- Components (1–2 sentences: button/card/input token usage)
- Do's and Don'ts (2–3 bullet points each)

### Step 4 — Validate and export

1. Verify WCAG contrast: primary on background ≥ 4.5:1, accent on background ≥ 4.5:1
2. Verify all `{token.references}` in components point to valid tokens
3. Generate `tokens.css` at the project root with CSS custom properties from the YAML tokens

Present the design system summary for user approval:

```
DESIGN SYSTEM SUMMARY:
- Mood: [Professional / Minimal / Bold / etc.]
- Accent: [color] — used for buttons, links, focus rings
- Font: [font family] — [size range]
- Dark mode: [Yes / No]
- Border radius: [Sharp / Subtle / Rounded]

→ Approve this design direction, or tell me what to change.
```

**Do NOT proceed until the user approves.**

---

## Edit Flow

### Step 1 — Read current DESIGN.md

Always read the existing `DESIGN.md` first.

### Step 2 — Make targeted changes

**Only modify what the user asked for. Preserve everything else.**

| User request | What to change | What to preserve |
|-------------|----------------|------------------|
| "Change accent to blue" | `colors.accent` value + Colors prose | Everything else |
| "Switch to Poppins font" | `fontFamily` in all typography entries + Typography prose | Sizes, weights, line heights |
| "Add dark mode" | Add `colors-dark` section | All existing tokens |
| "Make buttons more rounded" | `components.button.rounded` | Other component tokens |
| "Make headings larger" | `fontSize` in h1, h2, h3 | Font families, weights, body text |

**Rules:**
- Keep `---` YAML delimiters intact
- Keep `version` and `name` fields
- When changing a color, update the Colors prose section to match
- When changing typography, update the Typography prose section to match
- Use `{token.references}` in components where possible

### Step 3 — Validate and re-export

1. Verify the edited file is still valid YAML
2. Check WCAG contrast for any changed colors
3. Re-export `tokens.css` — all UI code that links to it will update automatically

---

## Rules

1. **Never hardcode colors, fonts, or spacing in UI code** — always use CSS variables from `tokens.css`
2. **Accent color is ONLY for interactive elements** — buttons, links, focus rings, toggles
3. **Verify WCAG contrast** — primary on background ≥ 4.5:1, accent on background ≥ 4.5:1
4. **Explain every choice** — tell the user WHY you picked a color, font, or spacing scale
5. **Preserve existing tokens** — when editing, only change what was requested

## Exit Criteria

- [ ] `DESIGN.md` created with complete YAML tokens
- [ ] All required color tokens defined (+ dark mode if requested)
- [ ] Typography scale covers 6+ levels
- [ ] Spacing scale covers xs–xl
- [ ] WCAG contrast ratios verified
- [ ] `tokens.css` exported with CSS custom properties
- [ ] User has reviewed and approved
