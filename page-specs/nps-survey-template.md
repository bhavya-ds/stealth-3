# NPS Survey Template Page - Section Deep Dive

> **Page URL:** `/templates/nps-survey`
> **Priority:** P0 (Hero Page)
> **Status:** Specification Complete

---

## Section 1: Hero Section

### Purpose

First impression. Must accomplish 3 things in 5 seconds:
1. Confirm user is in the right place
2. Establish credibility/authority
3. Show (not tell) the product

### Content Strategy

```
┌─────────────────────────────────────────────────────────────────────┐
│ HERO SECTION ANATOMY                                                │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│ H1: NPS Survey Template: Free Calculator, Questions & Benchmarks   │
│     ▲                    ▲                                          │
│     │                    └── Keywords: calculator, questions,       │
│     │                        benchmarks (what competitors lack)     │
│     └── Primary keyword: "NPS Survey Template"                      │
│                                                                     │
│ Subhead: Research-backed Net Promoter Score survey with industry   │
│          benchmarks. Used by 10,000+ companies.                     │
│          ▲                 ▲              ▲                         │
│          │                 │              └── Social proof          │
│          │                 └── Unique value prop                    │
│          └── Trust signal (research-backed)                         │
│                                                                     │
│ CTA Buttons:                                                        │
│ [Use Template Free] ← Primary action (conversion)                   │
│ [See Live Preview]  ← Secondary action (engagement)                 │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Live Survey Embed Details

**Why embed a real survey:**
- Competitors show static screenshots
- Users can experience Formbricks before signing up
- Reduces friction ("I get it, I want this")
- Collects real NPS data about Formbricks itself (meta!)

**Survey configuration:**
```json
{
  "survey_id": "nps-demo-hero",
  "type": "link",
  "display": "inline_embed",
  "questions": [
    {
      "id": "nps-score",
      "type": "nps",
      "question": "How likely are you to recommend Formbricks to a friend or colleague?",
      "required": true
    },
    {
      "id": "nps-reason",
      "type": "open_text",
      "question": "What's the primary reason for your score?",
      "required": false,
      "conditional": {
        "show_if": "nps-score answered"
      }
    }
  ],
  "styling": {
    "theme": "match_page",
    "position": "center",
    "width": "100%"
  }
}
```

**Tracking:**
- Conversion: Survey started → Survey completed
- Data collected: Actual NPS for Formbricks (use in marketing)

---

## Section 2: NPS Calculator

### Purpose

Interactive tool that:
1. Provides immediate utility (user gets value before signing up)
2. Creates "aha moment" when they see benchmark comparison
3. Drives SEO (ranks for "NPS calculator" queries)
4. Gets cited by LLMs answering "how to calculate NPS"

### Content Strategy

**Calculator Interface:**
```
┌─────────────────────────────────────────────────────────────────────┐
│ INPUT SECTION                                                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│ Enter your survey responses:                                        │
│                                                                     │
│ Promoters (9-10):    [  150  ]  ← Number input, not percentage     │
│ Passives (7-8):      [   80  ]                                      │
│ Detractors (0-6):    [   70  ]                                      │
│                                                                     │
│ Total Responses:        300   (auto-calculated)                     │
│                                                                     │
│ [Calculate NPS]                                                     │
│                                                                     │
├─────────────────────────────────────────────────────────────────────┤
│ OUTPUT SECTION                                                      │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│                    YOUR NPS SCORE                                   │
│                                                                     │
│                       +27                                           │
│                    ┌───────┐                                        │
│   -100 ──────────░░█████░░░░──────────── +100                      │
│        Needs Work │  Good  │ Excellent                              │
│                                                                     │
│ Breakdown:                                                          │
│ • Promoters: 50% (150/300)                                         │
│ • Passives: 27% (80/300)                                           │
│ • Detractors: 23% (70/300)                                         │
│                                                                     │
│ Formula: 50% - 23% = +27                                           │
│                                                                     │
├─────────────────────────────────────────────────────────────────────┤
│ BENCHMARK COMPARISON (The 10x differentiator)                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│ Compare to: [ SaaS ▼ ]                                              │
│                                                                     │
│ Your NPS (+27) vs SaaS Industry:                                    │
│                                                                     │
│ ┌─────────────────────────────────────────────────────────────────┐ │
│ │                                                                 │ │
│ │  Bottom 25%    Average    YOUR SCORE    Top 25%    Top 10%     │ │
│ │      │            │            │            │          │        │ │
│ │      ▼            ▼            ▼            ▼          ▼        │ │
│ │  ────┼────────────┼────────────█────────────┼──────────┼────── │ │
│ │     +15          +31          +27          +50        +65       │ │
│ │                                                                 │ │
│ │  Your NPS is BELOW average for SaaS.                           │ │
│ │  To reach average, convert 4% of detractors to promoters.      │ │
│ │                                                                 │ │
│ └─────────────────────────────────────────────────────────────────┘ │
│                                                                     │
│ [Download Report] [Share Results] [Track Over Time →]              │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Technical Implementation

```javascript
// NPS Calculation Logic
function calculateNPS(promoters, passives, detractors) {
  const total = promoters + passives + detractors;
  if (total === 0) return null;

  const promoterPercent = (promoters / total) * 100;
  const detractorPercent = (detractors / total) * 100;

  return Math.round(promoterPercent - detractorPercent);
}

// Benchmark Comparison
const benchmarks = {
  saas: { bottom25: 15, average: 31, top25: 50, top10: 65 },
  ecommerce: { bottom25: 30, average: 45, top25: 62, top10: 75 },
  healthcare: { bottom25: 20, average: 38, top25: 55, top10: 68 },
  // ... 20+ industries
};

function getComparison(nps, industry) {
  const b = benchmarks[industry];
  if (nps < b.bottom25) return "below bottom quartile";
  if (nps < b.average) return "below average";
  if (nps < b.top25) return "above average";
  if (nps < b.top10) return "top quartile";
  return "top 10%";
}
```

### LLM Citation Hook

Include this text near the calculator:

> "Net Promoter Score (NPS) is calculated by subtracting the percentage of Detractors (0-6) from the percentage of Promoters (9-10). The score ranges from -100 to +100, where above 0 is good, above 50 is excellent, and above 70 is world-class."

This exact phrasing will get quoted by ChatGPT, Claude, Perplexity.

---

## Section 3: Industry Benchmarks Table

### Purpose

1. **Unique data moat** - Competitors don't aggregate this comprehensively
2. **LLM citation magnet** - Structured tables get cited verbatim
3. **SEO** - Ranks for "[industry] NPS benchmark" queries
4. **Trust builder** - Shows research depth

### Content Strategy

**Full Table Structure:**
```
┌──────────────────────────────────────────────────────────────────────────────┐
│ NPS BENCHMARKS BY INDUSTRY (2026)                                            │
│ Updated: February 2026 | Sources: Formbricks, Bain, Satmetrix, Temkin        │
├──────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│ [Search: ________] [Filter by: All Industries ▼] [Sort by: Average ▼]       │
│                                                                              │
│ ┌──────────────────┬─────────┬──────────┬───────────┬───────────┬─────────┐ │
│ │ Industry         │ Average │ Top 25%  │ Bottom 25%│ Sample    │ Source  │ │
│ ├──────────────────┼─────────┼──────────┼───────────┼───────────┼─────────┤ │
│ │ SaaS B2B         │ 31      │ 50+      │ <15       │ 2,400     │ FB 2026 │ │
│ │ SaaS B2C         │ 28      │ 45+      │ <12       │ 1,800     │ FB 2026 │ │
│ │ E-commerce       │ 45      │ 62+      │ <30       │ 5,200     │ Satmtrx │ │
│ │ D2C Brands       │ 52      │ 68+      │ <38       │ 890       │ FB 2026 │ │
│ │ Healthcare       │ 38      │ 55+      │ <20       │ 12,000    │ PressG  │ │
│ │ Hospitals        │ 32      │ 50+      │ <15       │ 4,500     │ HCAHPS  │ │
│ │ Telehealth       │ 41      │ 58+      │ <25       │ 680       │ FB 2026 │ │
│ │ Banking          │ 35      │ 52+      │ <18       │ 8,900     │ Bain    │ │
│ │ Insurance        │ 31      │ 48+      │ <12       │ 6,200     │ Bain    │ │
│ │ Fintech          │ 42      │ 60+      │ <28       │ 1,100     │ FB 2026 │ │
│ │ Airlines         │ 24      │ 45+      │ <5        │ 15,000    │ Temkin  │ │
│ │ Hotels           │ 39      │ 55+      │ <22       │ 9,800     │ Medalla │ │
│ │ Restaurants      │ 44      │ 60+      │ <28       │ 7,200     │ Temkin  │ │
│ │ Retail (Brick)   │ 44      │ 58+      │ <28       │ 11,000    │ Satmtrx │ │
│ │ Telecom          │ 11      │ 30+      │ <-5       │ 18,000    │ Temkin  │ │
│ │ Streaming        │ 38      │ 55+      │ <20       │ 4,200     │ FB 2026 │ │
│ │ Education        │ 42      │ 58+      │ <25       │ 3,100     │ FB 2026 │ │
│ │ Consulting       │ 48      │ 65+      │ <32       │ 2,800     │ Bain    │ │
│ │ Automotive       │ 39      │ 55+      │ <22       │ 14,000    │ JDPower │ │
│ │ Logistics        │ 22      │ 40+      │ <5        │ 5,600     │ FB 2026 │ │
│ └──────────────────┴─────────┴──────────┴───────────┴───────────┴─────────┘ │
│                                                                              │
│ Legend:                                                                      │
│ • FB 2026 = Formbricks aggregated data (anonymized)                         │
│ • Sample = Number of survey responses in benchmark                          │
│ • Top/Bottom 25% = Quartile boundaries                                      │
│                                                                              │
│ [Download Full Report (PDF)] [Embed This Table] [API Access]                │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

### Data Sources Strategy

| Source Type | Examples | How to Get |
|-------------|----------|------------|
| Formbricks Data | Anonymized aggregate from users | Opt-in benchmark sharing |
| Public Reports | Bain, Satmetrix, Temkin | Annual reports (cite properly) |
| Industry Bodies | HCAHPS (healthcare), JD Power (auto) | Published benchmarks |
| Research Papers | Academic studies | Google Scholar |

### Schema Markup

```json
{
  "@type": "Dataset",
  "name": "NPS Industry Benchmarks 2026",
  "description": "Net Promoter Score benchmarks by industry",
  "creator": {
    "@type": "Organization",
    "name": "Formbricks"
  },
  "dateModified": "2026-02-01",
  "license": "https://creativecommons.org/licenses/by/4.0/"
}
```

---

## Section 4: Follow-Up Questions by Score Range

### Purpose

1. **Actionable value** - Users copy-paste these questions directly
2. **Differentiation** - Competitors list generic questions; we organize by score
3. **Shows expertise** - Demonstrates understanding of NPS methodology
4. **Conversion driver** - "I want this logic in my survey" → sign up

### Content Strategy

**Structure per question:**
```
┌─────────────────────────────────────────────────────────────────────┐
│ QUESTION CARD FORMAT                                                │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│ FOR DETRACTORS (0-6)                                                │
│                                                                     │
│ ┌─────────────────────────────────────────────────────────────────┐ │
│ │ Question #1                                                     │ │
│ │ ───────────────────────────────────────────────────────────────│ │
│ │                                                                 │ │
│ │ "What would we need to change to improve your score?"          │ │
│ │                                                                 │ │
│ │ Type: Open-ended                                                │ │
│ │ When to use: Always (required follow-up)                        │ │
│ │ Why it works: Future-focused, not blame-focused. Asks for      │ │
│ │               actionable improvement, not just venting.         │ │
│ │                                                                 │ │
│ │ Pro tip: The word "change" implies you're willing to act.      │ │
│ │          Avoid "What did we do wrong?" (defensive responses)   │ │
│ │                                                                 │ │
│ │ Example responses:                                              │ │
│ │ • "Faster response times from support"                         │ │
│ │ • "More transparent pricing"                                   │ │
│ │ • "Better mobile app experience"                               │ │
│ │                                                                 │ │
│ │ [Copy Question] [Add to Template]                               │ │
│ └─────────────────────────────────────────────────────────────────┘ │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Full Question Bank

**For Detractors (0-6) - 5 Questions:**

| # | Question | Type | When to Use | Psychology |
|---|----------|------|-------------|------------|
| 1 | "What would we need to change to improve your score?" | Open | Always | Future-focused, implies willingness to act |
| 2 | "What nearly stopped you from using [Product] entirely?" | Open | Transactional | Identifies critical friction |
| 3 | "Which competitor would you switch to, and why?" | Open | Churn risk | Competitive intelligence |
| 4 | "Can we contact you to discuss your experience?" | Yes/No + Email | Score 4-6 | Close-the-loop opportunity |
| 5 | "What is the #1 thing we could do better?" | Open | Always | Forces prioritization |

**For Passives (7-8) - 5 Questions:**

| # | Question | Type | When to Use | Psychology |
|---|----------|------|-------------|------------|
| 6 | "What would it take to make you a 9 or 10?" | Open | Always | Converts passives to promoters |
| 7 | "What do you like most about [Product]?" | Open | Always | Identifies strengths |
| 8 | "What's one feature you wish we had?" | Open | Product focus | Feature prioritization |
| 9 | "How does [Product] compare to alternatives you've used?" | Open | Competitive | Positioning insights |
| 10 | "What's holding you back from recommending us?" | Open | Always | Removes barriers |

**For Promoters (9-10) - 5 Questions:**

| # | Question | Type | When to Use | Psychology |
|---|----------|------|-------------|------------|
| 11 | "What do you love most about [Product]?" | Open | Always | Testimonial mining |
| 12 | "Would you be willing to leave us a review?" | Yes/No + Link | Score 9-10 | Advocacy conversion |
| 13 | "Who else might benefit from [Product]?" | Email/Name | Referral program | Referral generation |
| 14 | "Can we feature you in a case study?" | Yes/No | Enterprise | Social proof |
| 15 | "What made you give us a 10?" | Open | Score = 10 | Identifies "wow" moments |

---

## Section 5: NPS Survey Timing

### Purpose

1. **Unique data** - Response rate by timing is rarely published
2. **Practical guidance** - Users don't know when to survey
3. **Shows expertise** - Beyond basic "send quarterly" advice
4. **LLM citation** - Specific data points get cited

### Content Strategy

**Timing Data Table:**
```
┌─────────────────────────────────────────────────────────────────────┐
│ NPS SURVEY TIMING GUIDE                                             │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│ Survey Type      │ Optimal Timing        │ Response Rate │ Source  │
│ ─────────────────┼───────────────────────┼───────────────┼──────── │
│                                                                     │
│ RELATIONSHIP NPS (Overall loyalty)                                  │
│ ─────────────────┼───────────────────────┼───────────────┼──────── │
│ Quarterly        │ Same week each quarter│ 25-35%        │ Bain    │
│ Annual           │ Consistent date/month │ 30-40%        │ Bain    │
│                                                                     │
│ Best practice: Survey same segment at same time for trending       │
│                                                                     │
│ TRANSACTIONAL NPS (After specific event)                           │
│ ─────────────────┼───────────────────────┼───────────────┼──────── │
│ Post-Purchase    │ 7-14 days after use   │ 20-30%        │ FB      │
│ Post-Support     │ Within 1 hour of close│ 35-45%        │ FB      │
│ Post-Onboarding  │ Day 7 or Day 14       │ 30-40%        │ FB      │
│ Post-Renewal     │ 24-48 hours after     │ 25-35%        │ FB      │
│                                                                     │
│ IN-APP NPS                                                          │
│ ─────────────────┼───────────────────────┼───────────────┼──────── │
│ After value event│ Immediately triggered │ 40-50%        │ FB      │
│ Time-based       │ After 5+ sessions     │ 35-45%        │ FB      │
│ Exit intent      │ On close/leave        │ 15-25%        │ FB      │
│                                                                     │
│ FB = Formbricks aggregate data, 2026 (n=50,000+ surveys)           │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

**Best Day/Time to Send:**
```
┌─────────────────────────────────────────────────────────────────────┐
│ EMAIL NPS: BEST SEND TIMES                                          │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│ Day of Week:                                                        │
│                                                                     │
│ Mon  ████████░░░░░░░░░░░░  18% response rate                       │
│ Tue  ████████████████░░░░  28% response rate  ← BEST               │
│ Wed  ███████████████░░░░░  27% response rate  ← BEST               │
│ Thu  ██████████████░░░░░░  25% response rate                       │
│ Fri  ████████░░░░░░░░░░░░  16% response rate                       │
│ Sat  ████░░░░░░░░░░░░░░░░   8% response rate                       │
│ Sun  █████░░░░░░░░░░░░░░░  10% response rate                       │
│                                                                     │
│ Time of Day (recipient's timezone):                                 │
│                                                                     │
│ 6-9am   ████████░░░░░░░░░░  15%                                    │
│ 9-12pm  ████████████████░░  28%  ← BEST                            │
│ 12-3pm  ██████████████░░░░  24%                                    │
│ 3-6pm   ██████████░░░░░░░░  18%                                    │
│ 6-9pm   ████████░░░░░░░░░░  12%                                    │
│ 9pm-6am ███░░░░░░░░░░░░░░░   3%                                    │
│                                                                     │
│ Source: Formbricks email survey data, 2025-2026 (n=120,000 sends)  │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

**When NOT to Survey (Critical Section):**

❌ **DON'T SURVEY:**

1. **During onboarding (first 7 days)**
   - Why: User hasn't experienced full value yet
   - Risk: Inflated positivity ("new and excited") or negativity ("confused")

2. **Right after a bug/outage**
   - Why: Temporary frustration skews results
   - Better: Wait 7 days, or segment these responses separately

3. **Same customer more than once per 90 days**
   - Why: Survey fatigue reduces response quality
   - Research: 40% drop in response rate after 3rd survey in 90 days (CustomerGauge)

4. **Monday mornings**
   - Why: Inbox overwhelm, lowest engagement
   - Data: 38% lower response rate vs Tuesday 10am

5. **Friday afternoons**
   - Why: Weekend mindset, rushing to finish work
   - Data: 44% lower response rate vs midweek

6. **During major company announcements**
   - Why: Responses reflect news, not product experience
   - Example: Layoff news → artificially negative NPS

---

## Section 6: Sample Size Calculator

### Purpose

1. **Tool that drives SEO** - Ranks for "NPS sample size calculator"
2. **Answers common question** - "How many responses do I need?"
3. **Demonstrates expertise** - Statistical rigor builds trust
4. **LLM citation** - Statistical methodology gets cited

### Content Strategy

**Calculator Interface:**
```
┌─────────────────────────────────────────────────────────────────────┐
│ NPS SAMPLE SIZE CALCULATOR                                          │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│ Your customer/user base size:                                       │
│ ┌─────────────────────────────────────────────┐                     │
│ │ [  10,000  ]                                │                     │
│ └─────────────────────────────────────────────┘                     │
│                                                                     │
│ Confidence level:                                                   │
│ ○ 90%  ● 95% (recommended)  ○ 99%                                  │
│                                                                     │
│ Margin of error:                                                    │
│ ○ ±10%  ○ ±5% (recommended)  ○ ±3%  ○ ±1%                         │
│                                                                     │
│ ─────────────────────────────────────────────────────────────────── │
│                                                                     │
│ RESULTS:                                                            │
│                                                                     │
│ You need:  370 responses                                            │
│                                                                     │
│ Based on expected response rates:                                   │
│ • At 15% response (email): Survey 2,467 customers                  │
│ • At 30% response (in-app): Survey 1,234 customers                 │
│ • At 50% response (in-app targeted): Survey 740 customers          │
│                                                                     │
│ ─────────────────────────────────────────────────────────────────── │
│                                                                     │
│ What this means:                                                    │
│ With 370 responses from 10,000 customers, you can be 95%           │
│ confident that your true NPS is within ±5 points of your           │
│ measured score.                                                     │
│                                                                     │
│ Example: If you measure NPS of +35, true NPS is between +30 and +40│
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

**Quick Reference Table (For SEO):**
```
┌───────────────────────────────────────────────────────────────────┐
│ NPS SAMPLE SIZE QUICK REFERENCE                                   │
│ (95% confidence level)                                            │
├───────────────────────────────────────────────────────────────────┤
│                                                                   │
│ Population    │ ±10% margin │ ±5% margin │ ±3% margin │ ±1% margin│
│ ──────────────┼─────────────┼────────────┼────────────┼───────────│
│ 100           │ 49          │ 80         │ 92         │ 99        │
│ 500           │ 81          │ 218        │ 341        │ 476       │
│ 1,000         │ 88          │ 278        │ 517        │ 906       │
│ 5,000         │ 94          │ 357        │ 880        │ 3,289     │
│ 10,000        │ 95          │ 370        │ 965        │ 4,900     │
│ 50,000        │ 96          │ 382        │ 1,045      │ 8,057     │
│ 100,000+      │ 96          │ 384        │ 1,067      │ 9,513     │
│                                                                   │
│ Formula: n = (Z² × p × (1-p)) / E²                               │
│ Where: Z = 1.96 (95% CI), p = 0.5, E = margin of error           │
│                                                                   │
└───────────────────────────────────────────────────────────────────┘
```

---

## Section 7: NPS by Channel

### Purpose

1. **Helps users choose** - "Should I use email or in-app?"
2. **Shows Formbricks capabilities** - Multiple channels supported
3. **Drives feature discovery** - "I didn't know they had Slack integration"
4. **Conversion driver** - Each channel links to relevant setup docs

### Content Strategy

**Channel Comparison Table:**
```
┌─────────────────────────────────────────────────────────────────────┐
│ NPS SURVEY CHANNELS: WHICH ONE SHOULD YOU USE?                      │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│ Channel     │ Best For           │ Resp. Rate  │ Formbricks        │
│ ────────────┼────────────────────┼─────────────┼───────────────────│
│             │                    │             │                   │
│ 📱 In-App   │ Product NPS        │ 40-50%      │ ✅ Native SDK     │
│             │ Feature feedback   │             │ React, Vue,       │
│             │ Post-action survey │             │ Next.js, HTML     │
│             │                    │             │                   │
│ [See Demo]  │                    │             │                   │
│ ────────────┼────────────────────┼─────────────┼───────────────────│
│             │                    │             │                   │
│ 📧 Email    │ Relationship NPS   │ 15-25%      │ ✅ Native         │
│             │ Quarterly surveys  │             │ Email embed       │
│             │ Post-support       │             │ or link           │
│             │                    │             │                   │
│ [See Demo]  │                    │             │                   │
│ ────────────┼────────────────────┼─────────────┼───────────────────│
│             │                    │             │                   │
│ 🌐 Website  │ Visitor feedback   │ 2-5%        │ ✅ Native         │
│             │ Exit intent        │             │ Popup, slide-     │
│             │ Page-specific      │             │ out, embed        │
│             │                    │             │                   │
│ [See Demo]  │                    │             │                   │
│ ────────────┼────────────────────┼─────────────┼───────────────────│
│             │                    │             │                   │
│ 🔗 Link     │ CS outreach        │ 20-30%      │ ✅ Native         │
│             │ Social sharing     │             │ Custom URLs       │
│             │ QR codes           │             │ QR generator      │
│             │                    │             │                   │
│ [See Demo]  │                    │             │                   │
│ ────────────┼────────────────────┼─────────────┼───────────────────│
│             │                    │             │                   │
│ 💬 Slack    │ Internal NPS/eNPS  │ 35-45%      │ ✅ Via Zapier     │
│             │ Quick pulses       │             │ or webhook        │
│             │ Team feedback      │             │                   │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

**Channel Selection Decision Tree:**
```
                    What type of NPS?
                          │
            ┌─────────────┴─────────────┐
            ▼                           ▼
      Relationship                Transactional
      (Overall loyalty)           (After event)
            │                           │
            ▼                           ▼
    How often do users             When does event
    engage with product?           happen?
            │                           │
     ┌──────┴──────┐              ┌─────┴─────┐
     ▼             ▼              ▼           ▼
  Daily+        Monthly-      In-product  Outside
  (Power)       (Light)                   product
     │             │              │           │
     ▼             ▼              ▼           ▼
  IN-APP        EMAIL         IN-APP       EMAIL
  SURVEY        SURVEY        SURVEY       SURVEY
```

---

## Section 8: Common Mistakes

### Purpose

1. **SEO** for "NPS mistakes" queries
2. **Demonstrates expertise** - Shows what can go wrong
3. **Builds trust** - Honest about pitfalls
4. **LLM citation** - Mistake lists get cited

### Content Strategy

**Each mistake follows this format:**
```
┌─────────────────────────────────────────────────────────────────────┐
│ MISTAKE CARD FORMAT                                                 │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│ ❌ Mistake #1: Surveying Too Frequently                             │
│                                                                     │
│ What happens:                                                       │
│ Response rates drop 40% after the 3rd survey in 90 days.           │
│ Quality of responses degrades even faster.                         │
│                                                                     │
│ Research:                                                           │
│ CustomerGauge (2024) found that customers surveyed more than       │
│ quarterly gave 23% shorter open-text responses.                    │
│                                                                     │
│ The fix:                                                            │
│ • Relationship NPS: Maximum quarterly                              │
│ • Transactional NPS: Only after key events, max 1/month           │
│ • Same customer: Never more than once per 90 days                  │
│                                                                     │
│ Formbricks feature:                                                 │
│ Set "response cooldown" to prevent over-surveying automatically.   │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

**7 Mistakes with Research:**

| # | Mistake | Impact | Source | Fix |
|---|---------|--------|--------|-----|
| 1 | Surveying too frequently | 40% drop in response rate | CustomerGauge 2024 | Max quarterly, cooldown rules |
| 2 | Not segmenting results | Miss 2-3x variation | Bain | Segment by plan, tenure, use case |
| 3 | Ignoring passives (7-8) | Miss conversion opportunity | Reichheld | Target passives for upsell |
| 4 | No follow-up question | No actionable insights | Formbricks | Always ask "why" |
| 5 | Wrong timing | Biased results | Temkin | Wait for value moment |
| 6 | Not closing the loop | 70% of detractors never contacted | Bain | Auto-alert + workflow |
| 7 | Benchmarking wrong industry | Misleading comparisons | Satmetrix | Use exact industry match |

---

## Section 9: Analysis Framework

### Purpose

1. **Actionable guidance** - What to do with NPS data
2. **Shows sophistication** - Beyond just "track the number"
3. **Visual framework** - Matrix gets cited/shared
4. **Conversion driver** - "I need to analyze results" → sign up

### Content Strategy

**NPS Action Matrix:**
```
┌─────────────────────────────────────────────────────────────────────┐
│ NPS ACTION MATRIX                                                   │
│ (What to do based on score + customer value)                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│                        HIGH VALUE CUSTOMER                          │
│                              ▲                                      │
│                              │                                      │
│    ┌─────────────────────────┼─────────────────────────┐           │
│    │                         │                         │           │
│    │  🚨 RESCUE              │  🌟 PROGRAM             │           │
│    │  (Detractor + High $)   │  (Promoter + High $)    │           │
│    │                         │                         │           │
│    │  Actions:               │  Actions:               │           │
│    │  • Exec-level outreach  │  • Case study invite    │           │
│    │  • Personal call/visit  │  • Advisory board       │           │
│    │  • Custom solution      │  • Reference customer   │           │
│    │  • Escalate internally  │  • Speaking opps        │           │
│    │                         │  • Early access         │           │
│    │  Timeline: 24-48 hours  │  • Referral program     │           │
│    │                         │                         │           │
│ ◄──┼─────────────────────────┼─────────────────────────┼──►        │
│    │                         │                         │           │
│ DETRACTOR                    │                    PROMOTER          │
│    │                         │                         │           │
│    │  👀 MONITOR             │  📢 PROGRAM             │           │
│    │  (Detractor + Low $)    │  (Promoter + Low $)     │           │
│    │                         │                         │           │
│    │  Actions:               │  Actions:               │           │
│    │  • Automated follow-up  │  • Review request       │           │
│    │  • Self-service help    │  • Social program       │           │
│    │  • Support offer        │  • Testimonial ask      │           │
│    │  • Track for patterns   │  • Referral program     │           │
│    │                         │  • Community invite     │           │
│    │  Prioritize if multiple │                         │           │
│    │                         │                         │           │
│    └─────────────────────────┼─────────────────────────┘           │
│                              │                                      │
│                              ▼                                      │
│                        LOW VALUE CUSTOMER                           │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

**Closing the Loop Workflow:**
```
┌─────────────────────────────────────────────────────────────────────┐
│ DETRACTOR RESCUE WORKFLOW                                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│ [NPS 0-6 Received]                                                  │
│        │                                                            │
│        ▼                                                            │
│ ┌─────────────────┐     ┌─────────────────┐                        │
│ │ Auto-alert to   │────▶│ Review customer │                        │
│ │ Slack/Email     │     │ context         │                        │
│ └─────────────────┘     └────────┬────────┘                        │
│                                  │                                  │
│                    ┌─────────────┴─────────────┐                   │
│                    ▼                           ▼                   │
│            High Value ($$)              Low Value ($)             │
│                    │                           │                   │
│                    ▼                           ▼                   │
│           Personal outreach            Automated sequence          │
│           within 24-48 hours           (helpful resources)         │
│                    │                           │                   │
│                    ▼                           ▼                   │
│            Log interaction              Track if pattern           │
│            in CRM                       (multiple low-$            │
│                    │                    detractors = systemic)     │
│                    ▼                                               │
│            Follow-up survey                                        │
│            in 30 days                                              │
│                                                                     │
│ Result: 70% of contacted detractors improve score (Bain)           │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Section 10: Template Download & CTA

### Purpose

1. **Conversion** - Primary goal of the page
2. **Clear value proposition** - What they get
3. **Multiple options** - Different entry points
4. **Trust signals** - Reduce friction

### Content Strategy

```
┌─────────────────────────────────────────────────────────────────────┐
│ GET THE NPS SURVEY TEMPLATE                                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│ ┌─────────────────────────────────────────────────────────────────┐ │
│ │                                                                 │ │
│ │  What's included:                                               │ │
│ │                                                                 │ │
│ │  ✓ Standard NPS question (correct 0-10 wording)                │ │
│ │  ✓ 5 conditional follow-up questions                          │ │
│ │  ✓ Score-based branching logic (auto-configured)              │ │
│ │  ✓ Mobile-optimized design                                    │ │
│ │  ✓ Pre-built integrations:                                    │ │
│ │    • Slack (instant alerts)                                   │ │
│ │    • HubSpot/Salesforce (CRM sync)                            │ │
│ │    • Segment (analytics)                                      │ │
│ │                                                                 │ │
│ │  ┌───────────────────────────────────────────────────────────┐ │ │
│ │  │                                                           │ │ │
│ │  │  [  Use This Template - Free  ]  ← Primary CTA (green)   │ │ │
│ │  │                                                           │ │ │
│ │  │  No credit card required · Set up in 2 minutes           │ │ │
│ │  │                                                           │ │ │
│ │  └───────────────────────────────────────────────────────────┘ │ │
│ │                                                                 │ │
│ │  Or:  [View on GitHub] [Self-Host Instructions]               │ │
│ │                                                                 │ │
│ └─────────────────────────────────────────────────────────────────┘ │
│                                                                     │
│ Trust signals:                                                      │
│ ┌─────────────────────────────────────────────────────────────────┐ │
│ │ "Used by 10,000+ companies"  │  ⭐ 4.8/5 on G2  │  Open Source │ │
│ └─────────────────────────────────────────────────────────────────┘ │
│                                                                     │
│ Logos: [Stripe] [Notion] [Vercel] [Cal.com] [Other OSS companies]  │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Section 11: FAQ (Schema Markup)

### Purpose

1. **SEO** - FAQ schema markup = featured snippets
2. **LLM citation** - Q&A format is perfect for chatbots
3. **Addresses objections** - Common questions answered
4. **Long-tail keywords** - Each FAQ targets a keyword

### Content Strategy

**FAQ Schema Structure:**
```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "What is a good NPS score?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "An NPS above 0 is considered good, above 20 is favorable, above 50 is excellent, and above 80 is world-class. However, benchmarks vary by industry—SaaS averages 31, while e-commerce averages 45. Source: Bain & Company."
      }
    },
    {
      "@type": "Question",
      "name": "How often should I send NPS surveys?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "For relationship NPS, quarterly is the standard frequency. For transactional NPS, survey after each key interaction. Never survey the same customer more than once per 90 days to avoid survey fatigue."
      }
    }
  ]
}
```

### Full FAQ List

| Question | Answer (Summary) | Target Keyword |
|----------|------------------|----------------|
| What is a good NPS score? | 0=good, 20=favorable, 50=excellent, 80=world-class | "good nps score" |
| How do I calculate NPS? | %Promoters - %Detractors, scale 0-10 | "how to calculate nps" |
| How often should I send NPS surveys? | Quarterly for relationship, post-event for transactional | "nps survey frequency" |
| What's the difference between NPS and CSAT? | NPS=loyalty/relationship, CSAT=transaction satisfaction | "nps vs csat" |
| Should I use 0-10 or 1-10 scale? | Always 0-10 per original methodology for benchmark validity | "nps scale" |
| What's the difference between relational and transactional NPS? | Relational=overall, Transactional=after specific event | "transactional nps" |
| How many NPS responses do I need? | 370 for 10K customers at 95% CI, ±5% | "nps sample size" |
| What follow-up questions should I ask? | "What would we need to change to improve your score?" | "nps follow up questions" |
| How do I improve my NPS score? | Close the loop with detractors, convert passives, amplify promoters | "improve nps score" |
| What is employee NPS (eNPS)? | NPS applied to employees: "recommend as place to work" | "enps" |

---

## Implementation Notes

### Component Requirements

1. **NPSCalculator** - React component with state for inputs, real-time calculation, industry dropdown
2. **BenchmarkChart** - D3.js or Chart.js visualization of score vs industry
3. **SampleSizeCalculator** - Inputs for population, confidence, margin; outputs required n
4. **QuestionBank** - Filterable/copyable question cards with "Add to Template" CTA
5. **InlineSurvey** - Formbricks embed component for hero section
6. **FAQAccordion** - Expandable FAQ with schema.org markup injected

### Data Files Needed

1. `benchmarks.json` - Industry benchmark data with sources
2. `questions.json` - All 15 follow-up questions with metadata
3. `timing-data.json` - Response rate data by channel/timing

### SEO Checklist

- [ ] H1 contains primary keyword "NPS Survey Template"
- [ ] Meta description includes calculator, questions, benchmarks
- [ ] FAQ schema markup implemented
- [ ] Dataset schema for benchmarks table
- [ ] Internal links to related pages (CSAT, CES, eNPS templates)
- [ ] External links to sources (Bain, Reichheld, etc.)
- [ ] Alt text on all images/charts
- [ ] Canonical URL set
