# AI Listing Agent – Git-Based Daily Report & Feedback Rules

---

## Purpose
Automated AI agent that runs daily, finds highly selective home listings matching strict criteria, generates a Markdown report, commits it to the associated git repository, and continuously improves using feedback written directly in the repo.

This document is the **single source of truth** for agent behavior, scoring, output format, and learning loop.

---

## Daily Schedule
- **Frequency:** Daily
- **Target Time:** 07:30 AM (America/New_York)
- **Delivery Mechanism:** Markdown file committed and pushed to `main`
- **Output Directory:** `reports/`

---

## Expected Repo Structure
```
.
├── reports/
│   └── YYYY-MM-DD.md
├── feedback/
│   └── inbox.md
├── agent/
│   ├── rules.md | rules.yaml
│   ├── weights.json
│   └── state.md | state.json
└── README.md
```

Minimum required:
- `reports/`
- Either `feedback/inbox.md` **or** a feedback section in the most recent report

---

## Data Sources
- MLS / IDX feed (preferred)
- Realtor-provided alerts
- Public listing sources (Zillow, Redfin, Realtor.com)
- Builder inventory / quick move-in pages

---

## HARD FILTERS (Reject if ANY fail)

### Price
- $290,000 – $375,000
- Priority if ≤ $360,000

### Home Requirements
- Bedrooms: 3–4
- Bathrooms: ≥ 2.5
- Square Footage: ≥ 1,700 sqft
- HVAC: Central air required
- Garage: Required (2-car preferred)
- Office/Flex Space: Required

### Outdoor Living (MANDATORY)
Listing must include at least ONE:
- Carolina room
- Screened porch
- Enclosed porch or enclosed deck

**Open or uncovered deck only = REJECT**

---

## LOCATION LOGIC

### Always Include
- Carolina Lakes (Sanford, NC)
- Broadway, NC
- Angier, NC

### Conditional Include
- Cameron, NC
- Vass, NC
- Whispering Pines area
(Only if ALL hard filters are met)

### Exclude
- Fuquay-Varina unless price/value is exceptional

---

## PROPERTY PRIORITY SIGNALS
Increase score if listing meets any:
1. New construction (0–5 years)
2. Builder inventory / quick move-in
3. Back-on-market
4. Price reduction ≥ $10,000
5. Days on Market ≤ 5 AND price ≤ $360,000

---

## SIMILARITY BOOST (Benchmark Homes)
Boost score if similar to:
- 61 Lakewind Ct
- 124 Riviera Ln
- 812 Shady Ln
- Davidson-built homes in Angier

Similarity factors:
- Community amenities (pool, clubhouse)
- Comparable layout and square footage
- Similar price per square foot
- Cul-de-sac or low-traffic street

---

## AMENITY BONUS (Optional)
- HOA with active pool and clubhouse
- Established lot / mature landscaping
- Light or white kitchen finishes

---

## 5-POINT SCORECARD (Used for Ranking & Report Order)

Each listing is scored on a **0–5 scale**. Reports must be sorted **highest score → lowest score**.

### Score Components

**1. Hard Criteria Fit (0–1)**
- 1.0 = Meets ALL hard filters (required)
- 0.0 = Fails any hard filter (auto-reject)

**2. Price & Value Signal (0–1)**
- 1.0 = ≤ $360k OR price reduction ≥ $10k OR back-on-market
- 0.5 = $360k–$375k with strong features
- 0.0 = Overpriced for size/features

**3. Property Type Advantage (0–1)**
- 1.0 = New construction (≤5 yrs) OR builder inventory / quick move-in
- 0.5 = Competitive resale
- 0.0 = No value signal

**4. Location & Community Match (0–1)**
- 1.0 = Carolina Lakes / Broadway / Angier
- 0.5 = Cameron / Vass / Whispering Pines
- 0.0 = Outside zones

**5. Similarity & Lifestyle Bonus (0–1)**
- 1.0 = Strong similarity to benchmark homes + amenities
- 0.5 = Partial similarity
- 0.0 = No similarity

---

## SCORE INTERPRETATION
- **4.5 – 5.0** → 🥇 Strong Match
- **3.5 – 4.4** → 🥈 Review
- **< 3.5** → ❌ Reject (do not include)

---

## REPORT OUTPUT RULES

### File Naming
- `reports/YYYY-MM-DD.md`
- Never overwrite previous reports

### Required Sections
- Summary counts (🥇 / 🥈 / rejects)
- Listings in score order
- For each listing:
  - Address
  - Price
  - Beds / Baths / Sq Ft
  - Garage
  - Outdoor living confirmation
  - HOA / amenities
  - Final score
  - 1–2 bullet explanation
  - Listing link(s)

---

## GIT COMMIT RULES
- Commit new report daily
- Update learned files if applicable
- Commit message:
  - `Daily report: YYYY-MM-DD`
- Branch: `main`

---

## FEEDBACK LOOP (Repo-Based Learning)

### Feedback Sources (Priority Order)
1. `feedback/inbox.md`
2. Feedback section in most recent report
3. Existing stored preferences

### Supported Commands
```
LIKE: {address or listing id} (optional reason)
DISLIKE: {address or listing id} (optional reason)
RULE_ADD: {new rule}
EXCLUDE: {neighborhood / builder / street}
ADJUST: {weight change}
NOTE: {freeform guidance}
```

### Feedback Processing Rules
- Apply changes before next run
- Move processed commands to a `Processed` section
- Preserve audit trail with dates

---

## FEEDBACK SECTION TEMPLATE (Append to Every Report)

```md
---
## Feedback (read before next run)

### New Commands
LIKE:
DISLIKE:
RULE_ADD:
EXCLUDE:
ADJUST:
NOTE:

### Processed
- (agent-managed)
```

---

## OPERATING PRINCIPLES
- Precision > volume
- No speculative listings
- Covered outdoor living is mandatory
- Unknown data should downgrade or exclude
- Optimize for confidence, not completeness

---

## END OF DOCUMENT

