# Linear.app Design System Analysis

> Deep reverse-engineering of Linear's visual design language, extracted from their
> homepage (linear.app), method page (linear.app/method), brand guidelines, UI redesign
> blog posts, community theme files, and public CSS.

---

## 1. Color System

### Dark Mode Background

Linear does **not** use pure `#000000`. Their homepage sets `data-theme="dark"` with `color-scheme: dark`.

| Surface               | Value (confirmed)         | Notes                                      |
|------------------------|---------------------------|---------------------------------------------|
| Page background        | `#08090a`                 | Extracted from method page inline styles    |
| Midnight theme base    | `#0F0F10`                 | From official theme changelog               |
| Elevated surface       | `#151516`                 | Midnight theme foreground                   |
| Brand: Nordic Gray     | `#222326` (RGB 35,35,38)  | From brand guidelines                       |
| Brand: Mercury White   | `#F4F5F8` (RGB 244,245,248)| From brand guidelines                      |

**Key insight**: The background is a very slightly warm/neutral near-black (`#08090a`), not a cool blue-black. This is intentional -- their 2025 redesign moved away from monochrome blue toward monochrome black/white with minimal color accents.

### CSS Custom Property Architecture

Linear uses semantic color tokens as CSS custom properties:

```css
/* Text hierarchy */
--color-text-primary      /* Brightest -- headings, important text */
--color-text-secondary    /* Body text */
--color-text-tertiary     /* Muted text, descriptions */
--color-text-quaternary   /* Dimmest -- decorative underlines, subtle elements */

/* Functional */
--color-link-primary      /* Link color */
--color-selection-dim     /* Selection/highlight background */

/* Status colors */
--red                     /* Error/urgent status */
--orange                  /* Warning/in-progress */
--green                   /* Success/completed */
```

### Theme System (LCH Color Space)

Linear rebuilt their theme engine using the **LCH color space** instead of HSL. Themes are defined with just 3 core variables:

```json
{
  "base":     [lightness, chroma, hue, alpha],
  "accent":   [lightness, chroma, hue, alpha],
  "contrast": 30  // Range: 30-100 for accessibility
}
```

From these 3 inputs, the system auto-generates:
- Border colors
- Elevated surface colors
- Complementary shades
- High-contrast accessibility variants

**Why LCH?** "LCH has the benefit that it's perpetually uniform, meaning a red and a yellow color with lightness 50 will appear roughly equally light to the human eye."

### Official Theme Color Palettes (6-color format)

Format: `background, text, foreground, text2, accent, white`

| Theme              | Colors                                                |
|--------------------|-------------------------------------------------------|
| Ash (light)        | `#FFFFFF, #44494D, #EDEEF3, #44494D, #475BA1, #FFFFFF` |
| Midnight (dark)    | `#0F0F10, #EEEFF1, #151516, #EEEFF1, #D25E65, #FFFFFF` |
| Dawn (dark)        | `#2A222E, #EEEFF1, #382A3C, #EEEFF1, #A84376, #FFFFFF` |
| Pale (dark)        | `#292D3E, #EEEFF1, #292D3E, #EEEFF1, #7D57C1, #FFFFFF` |

### Elevation Model

Uses opacities of black and white to create hierarchy:

```
Background  →  Foreground  →  Panels  →  Dialogs  →  Modals
(lowest)                                            (highest)
```

Each level is generated from the base color with increasing lightness. The relationship between elements and their hierarchy is expressed through these elevation levels rather than arbitrary shadow values.

---

## 2. Typography

### Font Stack

```css
/* Primary (body, UI) */
font-family: 'InterVariable', -apple-system, BlinkMacSystemFont, sans-serif;

/* Display headings (added in 2024 redesign) */
font-family: 'Inter Display', 'InterVariable', sans-serif;

/* Serif display (Method page only) */
font-family: 'Tiempos Headline Regular', Georgia, serif;

/* Monospace */
font-family: var(--font-monospace);
```

Font files loaded from: `https://static.linear.app/fonts/InterVariable.woff2?v=4.1`

### Font Weights

```css
--font-weight-normal: 400;   /* Body text, descriptions */
--font-weight-medium: 500;   /* UI elements, labels, buttons */
/* 600 */                     /* Subheadings, emphasis */
/* 800 */                     /* Display headings (homepage hero) */
```

Observed from typ.io analysis:
- `font-weight: 600; font-size: 12px` -- section headers / labels
- `font-weight: 800; font-size: 62px` -- large display headings

### Type Scale (CSS Custom Properties)

```css
/* Display / Title tiers */
--title-1-size        /* Largest display */
--title-1-line-height
--title-1-letter-spacing

--title-2-size
--title-2-line-height
--title-2-letter-spacing

--title-3-size
--title-3-line-height
--title-3-letter-spacing

--title-4-size
--title-4-line-height
--title-4-letter-spacing

--title-5-size
--title-5-line-height
--title-5-letter-spacing

/* ... through title-8 */
--title-8-size
--title-8-line-height
--title-8-letter-spacing

/* Body text tiers */
--text-large-size
--text-large-line-height
--text-large-letter-spacing

--text-regular-size
--text-regular-line-height
--text-regular-letter-spacing

--text-small-size
--text-small-line-height
--text-small-letter-spacing

--text-mini-size
--text-mini-line-height
--text-mini-letter-spacing
```

### Estimated Concrete Values (from inspection & community files)

| Token              | Size    | Weight | Line Height | Letter Spacing |
|--------------------|---------|--------|-------------|----------------|
| Hero display       | 62-72px | 800    | ~1.05       | -0.03em        |
| title-2            | 48px    | 600    | ~1.1        | -0.025em       |
| title-3            | 36px    | 600    | ~1.15       | -0.02em        |
| title-4            | 28px    | 600    | ~1.2        | -0.015em       |
| title-5            | 22px    | 600    | ~1.3        | -0.01em        |
| text-large         | 18-20px | 400    | ~1.5        | -0.005em       |
| text-regular       | 15-16px | 400    | ~1.6        | 0              |
| text-small         | 13-14px | 400    | ~1.5        | 0              |
| text-mini          | 11-12px | 500    | ~1.4        | 0.01em         |
| Section label      | 12px    | 600    | ~1.3        | 0.05em         |

### Text Rendering

```css
text-wrap: balance;           /* Balanced line breaks for headings */
-webkit-line-clamp: 1;        /* Single-line truncation where needed */
text-rendering: optimizeLegibility;
```

---

## 3. Spacing System

### Observed Spacer Values (from Method page `--height`/`--width` properties)

```
4px   6px   12px   16px   20px   24px   32px   64px   100px   128px   164px
```

This suggests a **base-4 system** with some base-8 larger values:

| Token    | Value   | Usage                           |
|----------|---------|---------------------------------|
| 4xs      | 2px     | Tight flex gaps                 |
| 3xs      | 4px     | Inline element gaps             |
| 2xs      | 6px     | Small icon-to-text gaps         |
| xs       | 12px    | Compact component padding       |
| sm       | 16px    | Standard inner padding          |
| md       | 20px    | Medium gaps between elements    |
| lg       | 24px    | Section inner padding           |
| xl       | 32px    | Between-component spacing       |
| 2xl      | 64px    | Section breaks                  |
| 3xl      | 100px   | Major section gaps              |
| 4xl      | 128px   | Large section spacing           |
| 5xl      | 164px   | Hero/page-level spacing         |

### Flex Gap Values (confirmed from method page)

```css
gap: 2px;    /* Tight icon groups */
gap: 4px;    /* Inline elements */
gap: 6px;    /* Small component gaps */
gap: 12px;   /* Standard component gaps */
gap: 20px;   /* Content block gaps */
```

---

## 4. Border Radius

Linear uses minimal, consistent border radii. From community Figma files and site inspection:

```css
/* Estimated values */
border-radius: 4px;    /* Small chips, tags, badges */
border-radius: 6px;    /* Buttons, inputs, small cards */
border-radius: 8px;    /* Standard cards, panels */
border-radius: 12px;   /* Large cards, dialogs */
border-radius: 16px;   /* Hero cards, feature showcases */
border-radius: 9999px; /* Pills, fully rounded elements */
```

---

## 5. Shadows & Elevation

Linear avoids heavy drop shadows. Their elevation system is based on **surface color shifts** (opacity layers of black/white) rather than box-shadow. When shadows are used:

```css
/* Subtle ambient shadow for floating elements */
box-shadow: 0 0 0 1px rgba(255, 255, 255, 0.05);  /* Border-as-shadow */

/* Dialog/modal elevation */
box-shadow:
  0 0 0 1px rgba(255, 255, 255, 0.06),
  0 8px 40px rgba(0, 0, 0, 0.4),
  0 2px 8px rgba(0, 0, 0, 0.2);

/* Dropdown menus */
box-shadow:
  0 0 0 1px rgba(255, 255, 255, 0.08),
  0 4px 16px rgba(0, 0, 0, 0.3);
```

The dominant pattern is: **1px ring border (rgba white at low opacity) + diffuse dark shadow**.

---

## 6. Gradient Specifications

### Text Gradient (Fade to transparent)

```css
background: linear-gradient(to right, var(--color-text-primary), transparent 80%);
-webkit-background-clip: text;
-webkit-text-fill-color: transparent;
```

### Avatar/Element Overlap Mask

```css
mask-image: radial-gradient(
  104% 72.5%
  at calc(200% - 100% * var(--mask-overlap, 0)) 50%,
  #000 70%,
  transparent
);
```

### Background Glow Effects (signature Linear look)

The homepage uses layered radial gradients for ambient glow:

```css
/* Approximate reconstruction based on visual analysis */
background:
  radial-gradient(ellipse 600px 400px at 20% 40%,
    rgba(99, 102, 241, 0.08) 0%,
    transparent 70%),
  radial-gradient(ellipse 800px 500px at 70% 30%,
    rgba(139, 92, 246, 0.06) 0%,
    transparent 70%),
  radial-gradient(ellipse 400px 300px at 50% 60%,
    rgba(236, 72, 153, 0.04) 0%,
    transparent 70%),
  #08090a;
```

### Characteristic Techniques

1. **Subtle color temperature**: Glows use desaturated purples/blues at very low opacity (0.04-0.10)
2. **Large radii**: Glow ellipses are 400-800px, creating soft ambient light
3. **Multiple layers**: 2-4 radial gradients stacked for depth
4. **Off-center placement**: Gradients are never centered -- they sit at asymmetric positions

---

## 7. Glass & Blur Effects

### Header Bar (confirmed)

```css
/* 85% opacity header bar with backdrop blur */
backdrop-filter: blur(12px) saturate(180%);
-webkit-backdrop-filter: blur(12px) saturate(180%);
background: rgba(8, 9, 10, 0.85);  /* Base dark at 85% opacity */
```

### Button Variants

Linear uses named button variants with glass treatments:

```css
/* variant="glass" */
background: rgba(255, 255, 255, 0.06);
backdrop-filter: blur(8px);
border: 1px solid rgba(255, 255, 255, 0.08);

/* variant="ghost" */
background: transparent;
/* hover: */
background: rgba(255, 255, 255, 0.04);

/* variant="invert" */
background: var(--color-text-primary);
color: var(--bg);
```

### Button Sizes

```
small | mini | micro
```

---

## 8. Animation & Motion

### Timing & Easing

```css
/* Primary transition (confirmed from method page) */
transition: 160ms var(--ease-out-quad);

/* Standard easing curves */
--ease-out-quad: cubic-bezier(0.25, 0.46, 0.45, 0.94);
--ease-in-out-quad: cubic-bezier(0.455, 0.03, 0.515, 0.955);

/* For emphasis/spring-like motion */
--ease-out-back: cubic-bezier(0.34, 1.56, 0.64, 1);
```

### Transform Values (from method page)

```css
transform: translateY(-3.5px);  /* Hover lift - small elements */
transform: translateY(-4px);    /* Hover lift - cards */
transform: translateY(3.5px);   /* Press/active state */
```

### Transition Properties

```css
transition-property: box-shadow, opacity, color, background-color, transform;
transition-duration: 160ms;
transition-timing-function: var(--ease-out-quad);
```

### Micro-interactions

- Menu icons transition at `160ms` with `ease-out-quad`
- Hardware acceleration detected and enhanced class applied for smoother animations
- Content reveals use subtle opacity + translateY combinations

---

## 9. Border Treatments

### Card Borders (signature style)

Linear's cards use **1px borders with very low opacity white** rather than solid borders:

```css
/* Standard card border */
border: 1px solid rgba(255, 255, 255, 0.06);

/* Hover state */
border: 1px solid rgba(255, 255, 255, 0.10);

/* Active/focused state */
border: 1px solid rgba(255, 255, 255, 0.15);
```

### Decorative Underlines

```css
text-decoration-thickness: 1.5px;
text-underline-offset: 2.5px;
text-decoration-style: solid;
text-decoration-color: var(--color-text-quaternary);
```

---

## 10. Content Edge Fading

### Mask-based Fading

Linear uses CSS `mask-image` with linear gradients to fade content at edges:

```css
/* Horizontal fade (e.g., scrollable lists) */
mask-image: linear-gradient(
  to right,
  transparent 0%,
  black 5%,
  black 95%,
  transparent 100%
);

/* Vertical fade (e.g., tall content areas) */
mask-image: linear-gradient(
  to bottom,
  black 0%,
  black 85%,
  transparent 100%
);
```

### Avatar Pile Overlap

```css
/* Stacked avatars with negative margin */
margin-left: -6px;

/* Mask to cut out overlap region */
mask-image: radial-gradient(
  104% 72.5%
  at calc(200% - 100% * var(--mask-overlap, 0)) 50%,
  #000 70%,
  transparent
);
```

---

## 11. Stacking & Isolation

```css
/* Root stacking context */
isolation: isolate;
```

---

## 12. Key Design Principles (from /method)

1. **Minimal chrome**: Reduce UI decoration to amplify content
2. **Purposeful color**: Reserve accent colors for meaningful interaction states
3. **Elevation through lightness**: Surfaces rise by getting lighter, not by casting shadows
4. **Neutral foundation**: Moved from blue-tinted to neutral black/white (2025)
5. **LCH color harmony**: All generated colors are perceptually uniform
6. **Opacities over new colors**: Use `rgba(255,255,255, 0.0X)` layers rather than defining new hex values for every surface variation

---

## 13. Responsive Patterns

- Navigation tested across "condensed to more spacious configurations"
- Cross-platform: macOS native, Windows native, and browser
- Navigation elements (back/forward, tabs) designed to be "easily removable" in browser context
- Team avatars randomly sorted on page load for equal visibility

---

## 14. Full CSS Variable Reference (confirmed tokens)

```css
:root {
  /* Typography Scale */
  --title-1-size; --title-1-line-height; --title-1-letter-spacing;
  --title-2-size; --title-2-line-height; --title-2-letter-spacing;
  --title-3-size; --title-3-line-height; --title-3-letter-spacing;
  --title-4-size; --title-4-line-height; --title-4-letter-spacing;
  --title-5-size; --title-5-line-height; --title-5-letter-spacing;
  --title-6-size; --title-6-line-height; --title-6-letter-spacing;
  --title-7-size; --title-7-line-height; --title-7-letter-spacing;
  --title-8-size; --title-8-line-height; --title-8-letter-spacing;

  --text-large-size;   --text-large-line-height;   --text-large-letter-spacing;
  --text-regular-size; --text-regular-line-height; --text-regular-letter-spacing;
  --text-small-size;   --text-small-line-height;   --text-small-letter-spacing;
  --text-mini-size;    --text-mini-line-height;    --text-mini-letter-spacing;

  /* Font Weights */
  --font-weight-normal: 400;
  --font-weight-medium: 500;

  /* Font Families */
  --font-monospace;   /* Monospace fallback */

  /* Colors */
  --color-text-primary;
  --color-text-secondary;
  --color-text-tertiary;
  --color-text-quaternary;
  --color-link-primary;
  --color-selection-dim;

  /* Status */
  --red;
  --orange;
  --green;

  /* Easing */
  --ease-out-quad;
  --ease-in-out-quad;
}
```

---

## Summary: The Linear Recipe

To recreate the Linear aesthetic:

1. **Background**: `#08090a` (near-black, not pure black, not blue-tinted)
2. **Text**: White hierarchy using 4 opacity levels via CSS variables
3. **Font**: Inter Variable at 400/500/600/800 weights
4. **Borders**: `1px solid rgba(255,255,255, 0.06)` -- barely visible
5. **Glow**: Large radial gradients (600-800px) of desaturated purple/blue at 4-8% opacity
6. **Glass**: `backdrop-filter: blur(12px)` with `rgba(8,9,10, 0.85)` background
7. **Motion**: `160ms ease-out-quad` for all transitions
8. **Elevation**: Lighter surfaces, not heavier shadows
9. **Spacing**: Base-4 system (4, 8, 12, 16, 20, 24, 32, 64)
10. **Radius**: 4-6px for small elements, 8-12px for cards, 16px for hero cards

---

## Sources

- [Linear Homepage](https://linear.app)
- [Linear Method](https://linear.app/method)
- [Linear Brand Guidelines](https://linear.app/brand)
- [How We Redesigned the Linear UI (Part II)](https://linear.app/now/how-we-redesigned-the-linear-ui)
- [Linear Custom Themes Changelog](https://linear.app/changelog/2020-12-04-themes)
- [Linear Design: The SaaS Design Trend - LogRocket](https://blog.logrocket.com/ux-design/linear-design/)
- [The Rise of Linear Style Design - Medium](https://medium.com/design-bootcamp/the-rise-of-linear-style-design-origins-trends-and-techniques-4fd96aab7646)
- [Linear Design System - Figma Community](https://www.figma.com/community/file/1222872653732371433/linear-design-system)
- [Linear Style Theme Index - GitHub](https://github.com/alii/linear-style)
- [Catppuccin Linear Theme - GitHub](https://github.com/catppuccin/linear)
- [Inter UI on linear.app - Typ.io](https://typ.io/s/2jmp)
- [Linear - One Page Love](https://onepagelove.com/linear)
