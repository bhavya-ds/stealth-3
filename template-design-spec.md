# Formbricks pSEO Page Design Spec
## "Linear Personality x Formbricks Brand" Derivative System

> **Concept:** Linear's precision, generous spacing, minimal chrome, structured hierarchy, and smooth motion — applied to Formbricks' light-mode brand: white backgrounds, slate text, and teal accent. No dark mode.

---

## Design Philosophy

| Principle | Source | Application |
|-----------|--------|-------------|
| Minimal chrome | Linear | Content speaks; decoration reduced to thin borders + subtle shadows |
| Generous breathing room | Linear | 128px section spacing, 32px component gaps — content never feels cramped |
| Purposeful color | Linear | Teal accent ONLY for interactive elements + key data; everything else is neutral |
| Structured hierarchy | Linear | 4-level text hierarchy with tight letter-spacing on headings |
| Smooth precision | Linear | 160ms transitions, subtle hover lifts, spring easing for emphasis |
| Open source warmth | Formbricks | Friendly radius (8-12px), approachable copy, light/airy feel |
| Research authority | Formbricks pSEO | Data tables, citations, and calculators front-and-center |

---

## 1. COLOR PALETTE

### Backgrounds (Formbricks Light Mode)
| Token | Value | Use Case |
|-------|-------|----------|
| `--bg-page` | `#ffffff` | Page background |
| `--bg-elevated` | `#ffffff` | Card backgrounds (elevated with shadow, not color) |
| `--bg-surface` | `#f8fafc` | Secondary surfaces, alternating sections (slate-50) |
| `--bg-subtle` | `#f1f5f9` | Hover states, muted fills (slate-100) |
| `--bg-muted` | `#f4f6f8` | Soft gray for input backgrounds, code blocks |
| `--bg-input` | `#f8fafc` | Input field backgrounds |

### Text (Formbricks Slate Palette)
| Token | Value | Use Case |
|-------|-------|----------|
| `--text-primary` | `#0f172a` | Headlines, important text (slate-900) |
| `--text-secondary` | `#384258` | Body text, descriptions (Formbricks label-secondary) |
| `--text-muted` | `#64748b` | Captions, metadata, placeholders (slate-500) |
| `--text-faint` | `#94a3b8` | Decorative elements, disabled text (slate-400) |
| `--text-inverse` | `#ffffff` | Text on accent/dark backgrounds |

### Borders (Formbricks)
| Token | Value | Use Case |
|-------|-------|----------|
| `--border-default` | `#e0e0e0` | Card borders, dividers (Formbricks border-primary) |
| `--border-hover` | `#cbd5e1` | Hover state borders (slate-300) |
| `--border-focus` | `#038178` | Focus borders (brand teal) |
| `--border-accent` | `rgba(3,129,120, 0.25)` | Accent-tinted borders |

### Accent Colors (Formbricks Teal)
| Token | Value | Use Case |
|-------|-------|----------|
| `--accent` | `#038178` | Primary brand teal — CTAs, links, active states |
| `--accent-bright` | `#00E6CA` | Bright teal — highlights, decorative accents |
| `--accent-dark` | `#00C4B8` | Alternate teal for gradients |
| `--accent-soft` | `rgba(3,129,120, 0.08)` | Teal tinted backgrounds (badges, highlights) |
| `--accent-hover` | `#026b63` | Darker teal for button hover |

### Semantic Colors
| Token | Value | Use Case |
|-------|-------|----------|
| `--success` | `#16a34a` | Success states (green-600) |
| `--warning` | `#d97706` | Warning states (amber-600) |
| `--error` | `#dc2626` | Error states (red-600) |
| `--info` | `#2563eb` | Info states (blue-600) |

### NPS Score Colors
| Token | Value | Use Case |
|-------|-------|----------|
| `--score-promoter` | `#16a34a` | Promoter (9-10) — green |
| `--score-passive` | `#d97706` | Passive (7-8) — amber |
| `--score-detractor` | `#dc2626` | Detractor (0-6) — red |

---

## 2. TYPOGRAPHY

### Font Stack (Formbricks)
```css
--font-sans: 'Jost', ui-sans-serif, system-ui, -apple-system, sans-serif;
--font-display: 'Lexend', ui-sans-serif, system-ui, -apple-system, sans-serif;
--font-accent: 'Caveat', cursive;
--font-mono: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;
```

**Notes:**
- Jost is the primary body font (loaded with `font-feature-settings: 'ss01'` for alternate glyphs)
- Lexend is used for headings/display text (variable font, weight 100-900)
- Caveat is a handwritten accent font used sparingly for decorative callouts
- Mono uses system monospace stack (no custom mono font)

### Type Scale (Linear's precision, Formbricks' warmth)
| Token | Size | Weight | Line Height | Letter Spacing | Use Case |
|-------|------|--------|-------------|----------------|----------|
| `--display` | 56px | 800 | 1.05 | -0.03em | Hero headline |
| `--h1` | 40px | 700 | 1.1 | -0.025em | Section titles |
| `--h2` | 30px | 600 | 1.15 | -0.02em | Sub-section titles |
| `--h3` | 22px | 600 | 1.25 | -0.015em | Card/feature headers |
| `--h4` | 18px | 600 | 1.3 | -0.01em | Small headers |
| `--body-lg` | 18px | 400 | 1.6 | -0.005em | Lead paragraphs |
| `--body` | 15px | 400 | 1.65 | 0 | Default body text |
| `--body-sm` | 13px | 400 | 1.55 | 0 | Secondary text, table cells |
| `--caption` | 12px | 500 | 1.4 | 0.02em | Labels, metadata |
| `--overline` | 11px | 600 | 1.3 | 0.08em | Section labels, all-caps |

---

## 3. SPACING (Linear's Base-4)

| Token | Value | Use Case |
|-------|-------|----------|
| `--space-1` | 4px | Tight gaps |
| `--space-2` | 8px | Small element gaps |
| `--space-3` | 12px | Compact padding |
| `--space-4` | 16px | Standard inner padding |
| `--space-5` | 20px | Medium gaps |
| `--space-6` | 24px | Component padding |
| `--space-8` | 32px | Between-component gaps |
| `--space-10` | 40px | Card internal padding |
| `--space-12` | 48px | Sub-section gaps |
| `--space-16` | 64px | Section padding |
| `--space-20` | 80px | Major section breaks |
| `--space-24` | 96px | Page-level spacing |
| `--space-32` | 128px | Hero section padding |

---

## 4. BORDER RADIUS (Formbricks-friendly)

| Token | Value | Use Case |
|-------|-------|----------|
| `--radius-sm` | 6px | Buttons, inputs |
| `--radius-md` | 8px | Small cards, tooltips |
| `--radius-lg` | 12px | Standard cards |
| `--radius-xl` | 16px | Feature cards, panels |
| `--radius-2xl` | 20px | Hero cards |
| `--radius-full` | 9999px | Pills, avatars |

---

## 5. ELEVATION (Formbricks card shadows)

| Token | Value | Use Case |
|-------|-------|----------|
| `--elevation-0` | none | Flat elements |
| `--elevation-1` | `0 1px 3px rgba(15,23,42, 0.06), 0 0 0 1px rgba(15,23,42, 0.04)` | Subtle card |
| `--elevation-2` | `0 4px 16px rgba(15,23,42, 0.08), 0 0 0 1px rgba(15,23,42, 0.04)` | Hover/floating card |
| `--elevation-3` | `0 8px 40px rgba(15,23,42, 0.12), 0 0 0 1px rgba(15,23,42, 0.04)` | Modals, popovers |

---

## 6. MOTION (Linear's 160ms standard)

| Token | Value | Use Case |
|-------|-------|----------|
| `--duration-fast` | 160ms | Hovers, toggles |
| `--duration-normal` | 240ms | Transitions, reveals |
| `--duration-slow` | 400ms | Accordions, expansions |
| `--ease-out` | `cubic-bezier(0.25, 0.46, 0.45, 0.94)` | Standard easing |
| `--ease-spring` | `cubic-bezier(0.34, 1.56, 0.64, 1)` | Bouncy emphasis |

---

## 7. COMPONENT SPECS

### Navigation
```
Background: #ffffff with shadow --elevation-1
Height: 56px, sticky
Border-bottom: 1px solid --border-default
```

### Buttons
| Variant | Background | Text | Border |
|---------|------------|------|--------|
| Primary | `--accent` (#038178) | white | none |
| Primary hover | `--accent-hover` (#026b63) | white | none |
| Secondary | white | `--text-primary` | `--border-default` |
| Ghost | transparent | `--text-muted` | none |

### Cards
```
Background: white
Border: 1px solid --border-default
Radius: --radius-lg (12px)
Padding: 24px
Shadow: --elevation-1
Hover: --elevation-2
```

### Data Tables
```
Header bg: --bg-surface (#f8fafc)
Header text: --overline style
Row border: 1px solid #f1f5f9
Row hover: --bg-surface
```

---

## 8. CSS VARIABLES BLOCK

```css
:root {
  --bg-page: #ffffff;
  --bg-elevated: #ffffff;
  --bg-surface: #f8fafc;
  --bg-subtle: #f1f5f9;
  --bg-muted: #f4f6f8;
  --bg-input: #f8fafc;

  --text-primary: #0f172a;
  --text-secondary: #384258;
  --text-muted: #64748b;
  --text-faint: #94a3b8;
  --text-inverse: #ffffff;

  --border-default: #e0e0e0;
  --border-hover: #cbd5e1;
  --border-focus: #038178;
  --border-accent: rgba(3,129,120, 0.25);

  --accent: #038178;
  --accent-bright: #00E6CA;
  --accent-dark: #00C4B8;
  --accent-soft: rgba(3,129,120, 0.08);
  --accent-hover: #026b63;

  --success: #16a34a;
  --warning: #d97706;
  --error: #dc2626;
  --info: #2563eb;

  --score-promoter: #16a34a;
  --score-passive: #d97706;
  --score-detractor: #dc2626;

  --font-sans: 'Jost', ui-sans-serif, system-ui, -apple-system, sans-serif;
  --font-display: 'Lexend', ui-sans-serif, system-ui, -apple-system, sans-serif;
  --font-accent: 'Caveat', cursive;
  --font-mono: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;

  --space-1: 4px; --space-2: 8px; --space-3: 12px; --space-4: 16px;
  --space-5: 20px; --space-6: 24px; --space-8: 32px; --space-10: 40px;
  --space-12: 48px; --space-16: 64px; --space-20: 80px; --space-24: 96px;
  --space-32: 128px;

  --radius-sm: 6px; --radius-md: 8px; --radius-lg: 12px;
  --radius-xl: 16px; --radius-2xl: 20px; --radius-full: 9999px;

  --elevation-1: 0 1px 3px rgba(15,23,42, 0.06), 0 0 0 1px rgba(15,23,42, 0.04);
  --elevation-2: 0 4px 16px rgba(15,23,42, 0.08), 0 0 0 1px rgba(15,23,42, 0.04);
  --elevation-3: 0 8px 40px rgba(15,23,42, 0.12), 0 0 0 1px rgba(15,23,42, 0.04);

  --duration-fast: 160ms;
  --duration-normal: 240ms;
  --duration-slow: 400ms;
  --ease-out: cubic-bezier(0.25, 0.46, 0.45, 0.94);
  --ease-spring: cubic-bezier(0.34, 1.56, 0.64, 1);

  --max-width: 1120px;
  --side-padding: 40px;
}
```

---

## 9. DESIGN CHARACTER

**Light, precise, and trustworthy.** Linear's structural discipline — the tight letter-spacing, generous whitespace, 4-level text hierarchy, and smooth transitions — applied to a warm, light Formbricks canvas. Teal is the only chromatic color outside of semantic states. Everything else is white, slate gray, and shadows.
