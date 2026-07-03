---
name: trending
description: Discover newly emerging, newly launched, newly discussed, or rapidly accelerating trends with real-time momentum (today only). Return normalized, high-signal trend records for downstream rendering.
compatibility: Requires outbound HTTP access using native runtime networking (fetch/urllib/requests/etc). No additional dependencies required.
---

# Trend Radar (DATA LAYER ONLY)

## Purpose
Identify trends that are:
- newly released
- rapidly growing
- actively discussed today
- gaining measurable momentum

This layer ONLY handles:
- selection
- validation
- evidence aggregation
- ranking
- normalization

It MUST NOT handle:
- formatting
- language
- presentation structure

---

## CORE PRINCIPLE

This skill is a data-normalization layer.

Its job is NOT to write a polished report.
Its job IS to return stable, complete, downstream-ready trend records.

If a field cannot be supported by evidence or cautious inference from the same-day signals:
- leave it empty only when the schema explicitly allows it
- otherwise reject the item

Do not improvise extra sections.
Do not output commentary outside the final structured result.

---

## HARD TIME CONSTRAINT

Only include items with evidence occurring TODAY (user local time 00:00–23:59).

Valid triggers:
- official launch
- release
- announcement
- publication
- discussion spike (today)

Invalid:
- yesterday or earlier
- “recently popular” without timestamp
- trending without verifiable date

If fewer than 10 valid items exist → return fewer. Do NOT pad.

---

## GLOBAL DATA SOURCES (DO NOT LOCALIZE)

Use only raw global ecosystems:

- GitHub (release / spike required)
- Hacker News (today posts)
- Reddit (today posts)
- Product Hunt (today)
- arXiv / Hugging Face Papers
- Official blogs / company announcements
- Steam / YouTube (today content only)

DO NOT prefer any language or region.

---

## EVIDENCE REQUIREMENTS

Each trend MUST include:

- at least 1 verifiable source
- explicit publication date (YYYY-MM-DD)
- evidence must be TODAY
- a clearly attributable signal (release / launch / discussion spike / ranking / paper / announcement)

Reject if:

- no date
- uncertain timestamp
- inferred date
- non-verifiable source
- only generic popularity with no same-day trigger

---

## CROSS-SOURCE VALIDATION

Confidence levels:

- High: 3+ independent ecosystems
- Medium: 2 ecosystems
- Low: 1 ecosystem (avoid if possible)

Each ecosystem = GitHub / Reddit / HN / PH / arXiv

Different feeds within same platform do NOT count separately.

---

## RANKING PRIORITY

1. Recency (today validity)
2. Multi-source strength
3. Growth velocity
4. Practical usefulness
5. Novelty

Tie-break in this exact order:
1. official source present
2. more independent ecosystems
3. stronger same-day action signal (launch/release > ranking/post > discussion only)
4. clearer practical adoption path
5. alphabetical by normalized name

---

## DEDUP RULE

Merge by:
- exact name match
- URL match
- alias similarity

Prefer official project name + official URL.

---

## NORMALIZATION RULES

Each accepted item MUST be normalized before output.

### Name
- Use the official project / product / paper / release name.
- No marketing suffixes.
- No explanatory subtitle.

### Category
Use exactly one of:
- AI
- Developer Tools
- Infrastructure
- Security
- Data
- Research
- Productivity
- Consumer
- Creator
- Gaming
- Hardware
- Other

### Summary
- 1 sentence only
- 20–40 words preferred
- describe the concrete same-day signal plus what it is
- no hype language

### Why-it-matters
- 1 sentence only
- explain practical impact, not generic importance

### Suitable-audience
Use a compact comma-separated audience list.
Prefer roles such as:
- developers
- founders
- researchers
- security teams
- infra teams
- data teams
- creators
- gamers
- general users

### Trend-status
Use exactly one enum value:
- Emerging
- Growing
- Mainstream
- Declining

### Expected-duration
Use exactly one enum value:
- Flash
- Short
- Medium
- Long

Interpretation:
- Flash = days
- Short = 1–4 weeks
- Medium = 1–3 months
- Long = 3+ months

### Evidence-signals
- 1 to 3 entries only
- sort by strongest signal first
- each entry must represent a distinct source or ecosystem

### Official-URL
- Prefer homepage / official announcement / official repo
- Do not use mirrors, aggregators, or reposts when an official URL exists

---

## OUTPUT CONTRACT (DATA ONLY)

Return ONLY a structured array of trend objects in ranked order.

Preferred transport format: JSON array.

If no qualifying items exist:
- return an empty array
- do not add prose

Each object MUST contain these fields in this exact order:

1. name
2. category
3. summary
4. why_it_matters
5. suitable_audience
6. trend_status
7. expected_duration
8. confidence
9. official_url
10. evidence_signals

Field rules:

- name: string
- category: one enum from the category list
- summary: string
- why_it_matters: string
- suitable_audience: array of strings OR a comma-separated string; keep role-like labels only
- trend_status: one enum from the trend-status list
- expected_duration: one enum from the expected-duration list
- confidence: High | Medium | Low
- official_url: string
- evidence_signals: array with 1–3 objects

Each evidence_signals item MUST contain these fields in this exact order:

1. source
2. date
3. signal
4. url

Evidence field rules:

- source: platform or publisher name
- date: YYYY-MM-DD
- signal: concise factual note of the same-day event
- url: direct supporting URL

Hard constraints:

- max 10 items
- no duplicate items
- no Markdown
- no explanatory wrapper text
- no omitted required fields
- no fabricated dates, counts, rankings, or claims

---

## REJECTION RULES

Reject an item if ANY of the following is true:

- evidence is not from today
- no official or attributable source can be cited
- summary requires speculation to sound meaningful
- category cannot be assigned confidently
- official_url is missing when an official source exists but was not checked
- the item is merely evergreen popularity rather than a same-day trend trigger

---

## OUTPUT SEPARATION RULE

This skill MUST NOT define:
- Markdown layout
- final display labels
- output language
- headings or presentation prose

It ONLY returns normalized, presentation-agnostic trend records for a downstream output layer.
