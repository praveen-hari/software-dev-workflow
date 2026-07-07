# DESIGN.md Format Specification

Source: [google-labs-code/design.md](https://github.com/google-labs-code/design.md/blob/main/docs/spec.md)

DESIGN.md is a self-contained, plain-text representation of a design system. It has two parts:
- **YAML front matter** — machine-readable design tokens
- **Markdown body** — human-readable design rationale and guidance

The tokens are the normative values; the prose provides context for how to apply them.

---

## YAML Schema

```yaml
---
version: alpha
name: <string>
description: <string>      # optional

colors:
  <token-name>: <Color>    # any valid CSS color (hex recommended)

colors-dark:                # optional, for dark mode
  <token-name>: <Color>

typography:
  <token-name>:
    fontFamily: <string>
    fontSize: <Dimension>           # px, em, rem
    fontWeight: <number>            # 400, 700, etc.
    lineHeight: <Dimension|number>  # unitless multiplier recommended
    letterSpacing: <Dimension>      # optional
    fontFeature: <string>           # optional
    fontVariation: <string>         # optional

spacing:
  <scale-level>: <Dimension|number> # xs, sm, md, lg, xl

rounded:
  <scale-level>: <Dimension>        # sm, md, lg, full

components:
  <component-name>:
    backgroundColor: <Color|token-ref>
    textColor: <Color|token-ref>
    typography: <token-ref>         # reference to typography token
    rounded: <Dimension|token-ref>
    padding: <Dimension|token-ref>
    size: <Dimension>               # optional
    height: <Dimension>             # optional
    width: <Dimension>              # optional
---
```

### Token References

Use `{path.to.token}` syntax to reference other tokens:
```yaml
components:
  button-primary:
    backgroundColor: "{colors.accent}"
    textColor: "#FFFFFF"
    rounded: "{rounded.md}"
    padding: 12px
  button-primary-hover:
    backgroundColor: "{colors.accent-dark}"
```

### Color Formats

Hex (`#RRGGBB`) is recommended. Also valid: `rgb()`, `hsl()`, `oklch()`, `oklab()`, named colors, `color-mix()`.

---

## Markdown Sections (in order)

All sections use `##` headings. Omit sections that aren't relevant.

| # | Section | Purpose |
|---|---------|---------|
| 1 | **Overview** | Brand personality, mood, target audience |
| 2 | **Colors** | Color palettes with semantic roles |
| 3 | **Typography** | Font strategy, type scale rationale |
| 4 | **Layout** | Spacing strategy, grid model |
| 5 | **Elevation & Depth** | Shadow/depth strategy (or flat alternatives) |
| 6 | **Shapes** | Corner radius philosophy |
| 7 | **Components** | Component styling guidance (buttons, inputs, cards) |
| 8 | **Do's and Don'ts** | Practical guardrails |

### Section Guidelines

- Keep prose concise — 1–3 lines per section
- Prose may use descriptive color names ("Midnight Forest Green") that map to token names (`primary`)
- The YAML tokens are the source of truth, prose is context

---

## Recommended Token Names

**Colors:** `primary`, `secondary`, `tertiary`, `neutral`, `surface`, `on-surface`, `error`

**Typography:** `headline-display`, `headline-lg`, `headline-md`, `body-lg`, `body-md`, `body-sm`, `label-lg`, `label-md`, `label-sm`

**Rounded:** `none`, `sm`, `md`, `lg`, `xl`, `full`

---

## Minimum Requirements

| Category | Minimum |
|----------|---------|
| `colors` | At least `primary` defined |
| `typography` | 6+ levels (h1–caption or headline/body/label scale) |
| `spacing` | 5 levels (xs, sm, md, lg, xl) |
| `rounded` | 4 levels (sm, md, lg, full) |
| `components` | 3 components (button, card, input) |
| Markdown sections | At least 4 `##` headings |

## WCAG Contrast

- Normal text on background: minimum **4.5:1**
- Large text (18px+ or 14px+ bold): minimum **3:1**
- Interactive elements: minimum **3:1** against adjacent colors
