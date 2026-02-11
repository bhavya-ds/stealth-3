# NPS Survey Template Page - Implementation Spec

> **Page URL:** `/templates/nps-survey`
> **Priority:** P0 (Hero Page)
> **Status:** Implemented (v2 — Academic Research Rewrite)
> **File:** `nps-template.html`
> **Last Updated:** February 2026

---

## Research Foundation

This page was rebuilt from the ground up using findings from 52 peer-reviewed academic sources. Full report: `NPS_ACADEMIC_RESEARCH_REPORT.md`

### Key Academic Sources Driving Design Decisions

| Finding | Source | Impact on Page |
|---------|--------|----------------|
| 11-point scale exceeds psychometric optimum | Preston & Colman 2000 | FAQ addresses scale choice |
| Open text has low convergence with actual drivers | Bettencourt & Houston 2024 | Follow-up section leads with closed-ended driver chips |
| Mandatory open text increases breakoff | Hadler 2025 | Demo shows optional text step; Mistakes section warns against it |
| Chatbot surveys produce richer responses | Kim, Lee & Gweon 2019 (ACM CHI) | Channels section includes chatbot card |
| Personalized feedback +7pp response rate | Wenz et al. 2022 | Demo thank-you screen is conditional by segment |
| No progress bars = higher completion | Liu & Wronski 2018 | Design tips section; demo has no progress bar |
| Cultural response patterns across 26 countries | Harzing 2006 | Mistakes section warns about cross-cultural comparison |
| Mean score more stable than NPS formula | Eskildsen & Kristensen 2011 | Calculator shows mean alongside NPS |
| NPS alone insufficient — combine with CSAT/CES | De Haan et al. 2015 | Analysis section has "Don't Use NPS Alone" grid |
| Sliders increase cognitive load vs. radio buttons | Funke 2011 | Design tips; Mistakes section warns against sliders |

---

## Section 1: Hero Section

### What's Implemented
- H1: "NPS Survey Template" with subhead citing research-backed methodology
- 4-step interactive survey demo (not a single-question widget)
- Step indicator dots showing progress through flow

### 4-Step Survey Demo Flow

```
┌─────────────────────────────────────────────────────────────┐
│ STEP 1: Score (0-10)                                         │
│ "How likely are you to recommend us?"                        │
│ [0][1][2][3][4][5][6][7][8][9][10]                          │
│                                                              │
│ Labels: "Not at all likely" ←→ "Extremely likely"           │
│ Auto-segments: 0-6=Detractor, 7-8=Passive, 9-10=Promoter   │
│ After selection → 600ms delay → auto-advance to Step 2       │
├──────────────────────────────────────────────────────────────┤
│ STEP 2: Driver Chips (conditional by segment)                │
│                                                              │
│ Detractors see: "What disappointed you most?"                │
│   [Support Quality] [Product Bugs] [Pricing] [Onboarding]   │
│   [Missing Features] [Performance]                           │
│                                                              │
│ Passives see: "What would make you a 9 or 10?"              │
│   [Better Pricing] [More Features] [Faster Support]          │
│   [Better UX] [Integrations] [Reliability]                   │
│                                                              │
│ Promoters see: "What do you value most?"                     │
│   [Ease of Use] [Support Team] [Value for Money]             │
│   [Reliability] [Features] [Open Source]                     │
│                                                              │
│ Select up to 3 chips → [Next] button                         │
│ Research: Bettencourt & Houston 2024 — closed-ended drivers  │
│           converge better with actual churn/growth drivers    │
├──────────────────────────────────────────────────────────────┤
│ STEP 3: Open Text (optional)                                 │
│ "Anything else you'd like to share? (optional)"              │
│                                                              │
│ Textarea, placeholder: "Your feedback helps us improve..."   │
│ [Submit] button — no skip needed, text is optional           │
│ Research: Hadler 2025 — mandatory open text → breakoff       │
├──────────────────────────────────────────────────────────────┤
│ STEP 4: Thank You (conditional by segment)                   │
│                                                              │
│ Detractors: "Thank you. A team member will follow up         │
│              within 48 hours to address your concerns."       │
│                                                              │
│ Passives: "Thank you! We're always improving.                │
│            Here's what's coming next..."                      │
│                                                              │
│ Promoters: "You're amazing! Would you mind leaving us        │
│             a review?"                                        │
│                                                              │
│ [Start Over] button to reset demo                            │
│ Research: Wenz et al. 2022 — personalized feedback +7pp      │
└──────────────────────────────────────────────────────────────┘
```

### JavaScript State
- `demoState` object tracks: `score`, `segment`, `drivers[]`, `currentStep`
- `driverSets` object maps segment → question + chip options
- `thankYouMessages` object maps segment → personalized HTML
- `goToStep(n)` handles fadeIn animations and step dot updates
- `resetDemo()` returns to step 1

---

## Section 2: NPS Calculator

### What's Implemented
- 3 number inputs: Promoters (9-10), Passives (7-8), Detractors (0-6)
- Auto-calculated total
- Real-time NPS calculation with color-coded result
- Score bar visualization (-100 to +100)
- Percentage breakdown for each segment
- 95% confidence interval (shown when n ≥ 30)
- **Mean score display** (Eskildsen & Kristensen 2011) — estimates mean from grouped data
- Small sample warning (shown when n < 100)
- GEO passage block with citations (Reichheld 2003, Keiningham et al. 2007, Eskildsen & Kristensen 2011, Fisher & Kordupleski 2019)

### Benchmark Comparison (integrated into calculator card)
- "Compare to" dropdown with **16 industries** (all matching the benchmark table)
- 4-column grid: Bottom 25% | You | Average | Top 25%
- Dynamic summary text comparing user's NPS to selected industry
- Source attribution inline (e.g., "Source: Bain & Company")
- Changing the calculator updates the "You" box and summary
- Changing the dropdown updates all 4 boxes and recalculates summary

### Benchmark Data (16 industries)

| Key | Label | Avg | Top 25% | Bottom 25% | Source |
|-----|-------|-----|---------|------------|--------|
| saas_b2b | SaaS B2B | +31 | +50 | +15 | Formbricks 2026 |
| saas_b2c | SaaS B2C | +28 | +45 | +12 | Formbricks 2026 |
| ecommerce | E-commerce | +45 | +62 | +30 | Satmetrix |
| d2c | D2C Brands | +52 | +68 | +38 | Formbricks 2026 |
| healthcare | Healthcare | +38 | +55 | +20 | Press Ganey |
| banking | Banking | +35 | +52 | +18 | Bain & Company |
| insurance | Insurance | +31 | +48 | +12 | Bain & Company |
| fintech | Fintech | +42 | +60 | +28 | Formbricks 2026 |
| airlines | Airlines | +24 | +45 | +5 | Temkin Group |
| hotels | Hotels | +39 | +55 | +22 | Medallia |
| telecom | Telecom | +11 | +30 | -5 | Temkin Group |
| education | Education | +42 | +58 | +25 | Formbricks 2026 |
| consulting | Consulting | +48 | +65 | +32 | Bain & Company |
| manufacturing | Manufacturing | +65 | +75 | +45 | Retently |
| automotive | Automotive | +39 | +55 | +22 | JD Power |
| logistics | Logistics | +22 | +40 | +5 | Formbricks 2026 |

---

## Section 3: Industry Benchmarks Table

### What's Implemented
- Full data table with 16 industries
- Columns: Industry, Average NPS, Trend (arrows), Top 25%, Bottom 25%, Spread, Source
- Source badges: teal "FB 2026" for Formbricks data, gray badges for external sources
- Cultural comparison callout (Harzing 2006): Japan averages 20-30 points lower than US due to cultural midpoint bias
- GEO passage with citations

---

## Section 4: Follow-Up Questions

### What's Implemented (Academic Research Rewrite)

**Structure changed from v1:**
- v1: Flat list of 15 open-text questions organized by segment
- v2: Research callout leading with closed-ended driver chips, then open-text as supplementary

**Current structure:**
1. Research callout: Bettencourt & Houston 2024 finding that open text has low convergence with actual drivers
2. 3-column grid showing recommended driver chips by segment (Detractors / Passives / Promoters)
3. Open-text questions organized as supplementary follow-ups below
4. GEO passage explaining closed-ended + open-ended combination approach

---

## Section 5: Methodology

### What's Implemented
- Academic-grade methodology section with 8+ citations
- Topics: original scale (Preston & Colman 2000), behavioral prediction (Morgan & Rego 2006), international validity (Romaniuk et al. 2011), statistical challenges (Diamantopoulos et al. 2012), NPS 3.0 (Reichheld 2021)
- Schlösser 2024 citation on measurement precision at category boundaries

---

## Section 6: Survey Timing

### What's Implemented
- Timing grid: Relationship NPS (quarterly/annual), Transactional NPS (post-purchase/support/onboarding), In-App NPS (value events)
- "When NOT to Survey" list
- **Academic Design Tips** (new in v2):
  - Put demographic questions last (Thau 2021)
  - Use radio buttons, not sliders (Funke 2011; Bosch et al. 2019)
  - Don't show progress bars (Liu & Wronski 2018)
  - Limit to 3-5 questions total (Brosnan et al. 2021)
- GEO passage with citations

---

## Section 7: Sample Size Calculator

### What's Implemented
- Population input
- Confidence level radio pills: 90%, 95% (default), 99%
- Margin of error radio pills: ±10%, ±5% (default), ±3%, ±1%
- Result: required sample size (Cochran's formula with finite population correction)
- Response rate estimates: Email (15%), In-app (30%), Targeted (50%)
- Quick reference table for SEO

---

## Section 8: Channels

### What's Implemented
- 6 channel cards in 3×2 grid (`.grid-6`):
  1. In-App (40-50% response rate)
  2. Email (15-25%)
  3. Website (2-5%)
  4. Link/QR (20-30%)
  5. Slack (35-45%)
  6. **Chatbot** (new in v2) — 25-35%, based on Kim, Lee & Gweon 2019 (ACM CHI)

---

## Section 9: Common Mistakes

### What's Implemented (Expanded from 7 to 10)

| # | Mistake | Source |
|---|---------|--------|
| 1 | Surveying too frequently | CustomerGauge |
| 2 | Not segmenting results | Bain |
| 3 | Ignoring passives | Reichheld |
| 4 | No follow-up question | Formbricks |
| 5 | Wrong timing | Temkin |
| 6 | Not closing the loop | Bain |
| 7 | Benchmarking wrong industry | Satmetrix |
| 8 | Using sliders instead of radio buttons | Funke 2011 |
| 9 | Making open text mandatory | Hadler 2025 |
| 10 | Comparing NPS across cultures without adjustment | Harzing 2006 |

---

## Section 10: Analysis Framework

### What's Implemented
- NPS Action Matrix (2×2: Score segment × Customer value)
- Detractor rescue workflow
- **"Don't Use NPS Alone"** section (new in v2):
  - 3-column grid: NPS vs CSAT vs CES
  - Each shows: what it measures, when to use, limitation
  - Based on De Haan et al. 2015 (Journal of the Academy of Marketing Science)

---

## Section 11: CTA

### What's Implemented
- "Use This Template — Free" primary CTA button
- Feature checklist (4-step flow, conditional logic, 16 industry benchmarks, etc.)
- Trust signals: 10,000+ companies, open source, no credit card
- Secondary CTA: "View on GitHub"

---

## Section 12: FAQ

### What's Implemented
- 13 questions with accordion toggle (expanded from 10 in v1)
- Schema markup: FAQPage with all 13 Q&A pairs
- **New questions added in v2:**
  - "Is the 0-10 scale the best choice?" (cites Preston & Colman 2000)
  - "What is NPS 3.0?" (cites Reichheld 2021)
  - "Should I use NPS alone?" (cites De Haan et al. 2015)

---

## Schema Markup

6 schema types implemented in `<script type="application/ld+json">`:

1. **FAQPage** — 13 questions
2. **HowTo** — 4 steps (Score → Drivers → Open text → Thank you)
3. **Dataset** — NPS Industry Benchmarks 2026
4. **SoftwareApplication** — Formbricks
5. **Article** — Page metadata
6. **Organization** — Formbricks

---

## Technical Details

### Fonts (Google Fonts)
- Lexend (display: 300-800 weight range)
- Jost (body: 300-700 weight range)
- Caveat (accent/handwritten)

### Design System
- Based on `template-design-spec.md` (Linear personality × Formbricks brand)
- Light mode only, teal accent (#038178), white/slate palette
- See `formbricks-design-tokens.md` for extracted Formbricks tokens

### JavaScript Features
- 4-step survey demo with conditional branching
- NPS calculator with CI, mean score, and dynamic benchmark comparison
- Sample size calculator (Cochran's formula + finite population correction)
- Industry benchmark dropdown (16 industries, updates all comparison boxes)
- FAQ accordion toggle
- Smooth scroll navigation

### File Dependencies
- Single self-contained HTML file (no external JS/CSS frameworks)
- Google Fonts loaded via `<link>`
- All CSS inline in `<style>` block
- All JS inline in `<script>` block at end of body
