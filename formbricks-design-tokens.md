# Formbricks Design Tokens - Extracted from GitHub Repository

## Source Files
- `apps/web/tailwind.config.js` - Main web app Tailwind config
- `apps/web/modules/ui/globals.css` - Global CSS custom properties
- `packages/survey-ui/tailwind.config.ts` - Survey UI Tailwind config
- `packages/survey-ui/src/styles/globals.css` - Survey UI design tokens
- `apps/web/lib/constants.ts` - Application constants

---

## Brand Colors

### Primary Brand Color (CSS Variable-based)
- `--formbricks-brand: #038178` (dark teal/green - used in globals.css)
- Brand light: `#00E6CA` (bright teal - tailwind.config.js)
- Brand dark: `#00C4B8` (darker teal - tailwind.config.js)
- Brand new (CSS var): `var(--formbricks-brand, #038178)`
- DEFAULT_BRAND_COLOR: `#64748b` (slate gray - from constants.ts, used as survey default)
- Survey brand color default: `--fb-survey-brand-color: #64748b`

### Primary Palette
- Primary DEFAULT: `#0f172a` (very dark slate/navy)
- Primary foreground: `#fefefe` (near-white)

### Secondary Palette
- Secondary DEFAULT: `#f1f5f9` (light gray/slate-100)
- Secondary foreground: `#0f172a`

### Accent Palette
- Accent DEFAULT: `#f4f6f8` (light gray background)
- Accent foreground: `#0f172a`

### Destructive Palette
- Destructive DEFAULT: `#FF6B6B` (coral red)
- Destructive foreground: `#FFF5F5`

### Functional Colors
- Focus: `--formbricks-focus: #1982fc` (bright blue)
- Error: `--formbricks-error: #d13a3a` (red)

### Fill Colors (Light Mode)
- `--formbricks-fill-primary: #fefefe`
- `--formbricks-fill-secondary: #0f172a`
- `--formbricks-fill-disabled: #e0e0e0`

### Fill Colors (Dark Mode)
- `--formbricks-fill-primary: #0f172a`
- `--formbricks-fill-secondary: #e0e0e0`
- `--formbricks-fill-disabled: #394258`

### Label Colors (Light Mode)
- `--formbricks-label-primary: #0f172a`
- `--formbricks-label-secondary: #384258`
- `--formbricks-label-disabled: #bdbdbd`

### Label Colors (Dark Mode)
- `--formbricks-label-primary: #fefefe`
- `--formbricks-label-secondary: #f2f2f2`
- `--formbricks-label-disabled: #bdbdbd`

### Border Colors (Light Mode)
- `--formbricks-border-primary: #e0e0e0`
- `--formbricks-border-secondary: #0f172a`
- `--formbricks-border-disabled: #ececec`

### Border Colors (Dark Mode)
- `--formbricks-border-primary: #394258`
- `--formbricks-border-secondary: #e0e0e0`
- `--formbricks-border-disabled: #394258`

### Semantic Colors (from Tailwind colors)
- Info: blue-600, blue-900, blue-700, blue-50, blue-100
- Warning: amber-500, amber-900, amber-700, amber-50, amber-100
- Success: green-600, green-900, green-700, green-50, green-100
- Error: red-600, red-900, red-700, red-50, red-100

### Survey Background Colors
```
#FFFFFF, #FFF2D8, #EAD7BB, #BCA37F, #113946,
#04364A, #176B87, #64CCC5, #DAFFFB, #132043,
#1F4172, #F1B4BB, #FDF0F0, #001524, #445D48,
#D6CC99, #FDE5D4, #BEADFA, #D0BFFF, #DFCCFB,
#FFF8C9, #FF8080, #FFCF96, #F6FDC3, #CDFAD5
```

---

## Fonts / Typography

### Web App Fonts
The Formbricks web app (`apps/web`) does NOT use custom font imports via `next/font` or external font packages. The app relies on Tailwind CSS default font stacks (system fonts). No `@font-face` declarations, Google Fonts imports, or font files were found.

The default Tailwind CSS font families apply:
- **Sans (body):** `ui-sans-serif, system-ui, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji"`
- **Mono (code):** `ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace`

### Survey UI Font Tokens (customizable via CSS variables)
- `--fb-element-headline-font-family: inherit`
- `--fb-element-description-font-family: inherit`
- `--fb-label-font-family: inherit`
- `--fb-button-font-family: inherit`
- `--fb-input-font-family: inherit`
- `--fb-option-font-family: var(--fb-input-font-family)`

All default to `inherit`, meaning they use the parent/system font stack.

### Typography Scale (from typography components)
- H1: `text-4xl font-bold tracking-tight` (text-slate-800)
- H2: `text-3xl font-semibold tracking-tight` (text-slate-800)
- H3: `text-lg font-medium` (text-slate-800)
- H4: `text-base tracking-tight` (text-slate-800)
- Lead: `text-xl`
- P: `leading-7`
- Large: `text-lg`
- Base: `text-base`
- Small: `text-sm leading-none`
- Muted: `text-sm text-muted-foreground`
- InlineCode: `font-mono text-sm font-semibold`

---

## Tailwind Theme Extensions

### Shadows
- card-sm: `0px 0.5px 12px -5px rgba(30,41,59,0.20)`
- card-md: `0px 1px 25px -10px rgba(30,41,59,0.30)`
- card-lg: `0px 2px 51px -19px rgba(30,41,59,0.40)`
- card-xl: `0px 20px 25px -5px rgba(0,0,0,0.10), 0px 10px 10px -5px rgba(0,0,0,0.04)`

### Blur
- xxs: `0.33px`
- xs: `2px`

### Border Radius
- Default radius: `0.625rem` (from survey-ui `--radius`)
- Input: `var(--fb-input-border-radius)`
- Option: `var(--fb-option-border-radius)`
- Button: `var(--fb-button-border-radius)`

### Custom Widths
- sidebar-expanded: `4rem`
- sidebar-collapsed: `14rem`

### Max Width
- 8xl: `88rem`

### Custom Screens
- xs: `430px`

### Grid
- 20-column grid: `repeat(20, minmax(0, 1fr))`

### Scale
- 97: `0.97`

### Animations
- ping-slow, shake, accordion-down/up, fadeIn/fadeOut, surveyLoading, surveyExit

### Plugins
- @tailwindcss/forms
- @tailwindcss/typography

### Dark Mode
- Strategy: `class`

### shadcn/ui Configuration
- Style: "new-york"
- Base color: slate
- CSS variables: enabled
