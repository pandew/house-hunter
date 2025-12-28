# AI Listing Agent – Daily Run & Feedback Loop Specification

---

## Purpose
Automated AI agent that runs daily, finds highly selective home listings matching strict criteria, emails results to **pandew@gmail.com**, and continuously improves using user feedback.

---

## Daily Schedule
- **Frequency:** Daily
- **Time:** 07:30 AM (America/New_York)
- **Delivery:** Email digest
- **Recipient:** pandew@gmail.com

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
- HVAC: Central air
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

## 5-POINT SCORECARD (Used for Ranking & Email Order)

Each listing is scored on a 0–5 scale. Email results must be sorted **highest score → lowest score**.

### Score Components

**1. Hard Criteria Fit (0–1 point)**
- 1 = Meets ALL hard filters (required to be included)
- 0 = Fails hard filters (auto-reject, do not email)

**2. Price & Value Signal (0–1 point)**
- 1.0 = ≤ $360k OR price reduction ≥ $10k OR back-on-market
- 0.5 = $360k–$375k with strong features
- 0 = Overpriced for size/features

**3. Property Type Advantage (0–1 point)**
- 1.0 = New construction (≤5 yrs) OR builder inventory / quick move-in
- 0.5 = Well-maintained resale with competitive DOM
- 0 = Older resale with no value signal

**4. Location & Community Match (0–1 point)**
- 1.0 = Carolina Lakes / Broadway / Angier (preferred community)
- 0.5 = Cameron / Vass / Whispering Pines (conditional area)
- 0 = Outside defined zones

**5. Similarity & Lifestyle Bonus (0–1 point)**
- 1.0 = Strong similarity to benchmark homes + amenities (pool, cul-de-sac, layout)
- 0.5 = Partial similarity
- 0 = No meaningful similarity

---

## SCORE INTERPRETATION
- **5.0 – 4.5** → 🥇 Strong Match (Top-tier, prioritize showing)
- **4.4 – 3.5** → 🥈 Review (Good fit, worth evaluation)
- **< 3.5** → ❌ Reject (Do not email)

---

## ALERT OUTPUT RULES

### Ranking Tags
- 🥇 **Strong Match** – Meets all hard filters + high similarity score
- 🥈 **Review** – Meets all hard filters, moderate similarity
- ❌ **Reject** – Fails hard filters (do not email listing)

### Email Must Include
- Address
- Price
- Beds / Baths / Sq Ft
- Garage type
- Outdoor living confirmation
- HOA / amenities
- Reason for match (1–2 bullets)
- Listing link

---

## DAILY EMAIL SUBJECT
```
Daily Home Match Report — {YYYY-MM-DD}
```

---

## FEEDBACK LOOP (Learning System)

### Accepted Feedback Methods
- Reply directly to daily email
- Form submission (optional enhancement)

### Supported Feedback Commands
```
LIKE: {Address or Listing ID}
DISLIKE: {Address or Listing ID} (optional reason)
RULE_ADD: {New rule}
EXCLUDE: {Neighborhood / Builder / Street}
ADJUST: {Preference or weight change}
```

### Examples
```
LIKE: 124 Riviera Ln
DISLIKE: 88 Sample Dr (backs to busy road)
RULE_ADD: Reject homes backing to main roads
EXCLUDE: XYZ Community
ADJUST: Increase weight for active pool +15
```

### Persistence
- Update hard rules
- Update scoring weights
- Update exclusions
- Apply changes before next daily run

---

## DATA STORAGE (Suggested)
- listings_seen
- preference_rules
- preference_weights
- feedback_log

---

## OPERATING PRINCIPLES
- Precision > volume
- No speculative listings
- No missing covered outdoor living
- Fewer, high-confidence alerts preferred

---

## TEMPLATE: DAILY FEEDBACK EMAIL (User Reply)

**Subject:** Re: Daily Home Match Report — {YYYY-MM-DD}

```
LIKE: 123 Example St
DISLIKE: 88 Sample Rd (lot too small)
RULE_ADD: Reject homes with only open decks
ADJUST: Increase weight for cul-de-sac +10
```

---

## END OF DOCUMENT

