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

Maximum = 10 valid items. 10 is a ceiling, not a quota.

If fewer than 10 valid items are found on the first pass:
- continue searching
- widen source coverage
- accept lower-confidence items only if they fully satisfy every rule

If the allowed source set is exhausted with fewer than 10 valid items, return fewer and explain why (see SHORTFALL RULE).
Do NOT pad with fabricated, date-invalid, or weaker items to reach 10.

---

## TARGET COUNT RULE

The output holds at most 10 items. Reaching 10 is not required.

Run the search as a loop, not a single pass:

1. Collect same-day candidates from ladder step 1 onward. Do not apply rejection rules while collecting; keep every candidate that has a source URL and a date.
2. After each ladder step, validate the collected candidates against the EVIDENCE, CROSS-SOURCE, and REJECTION rules and count the accepted items.
3. If accepted items >= 10, stop searching and return the top 10 by ranking.
4. If accepted items < 10, continue to the next ladder step.

Do not conclude that fewer items exist before ladder step 7 is complete.

Priority order:
1. Every item fully validated; never pad
2. Up to 10 items; fewer is allowed only when the ladder is exhausted, with shortfall_reason set

Valid low-confidence items are acceptable near the bottom of the list only if they still satisfy:
- same-day evidence
- verifiable source
- attributable signal
- non-duplicative value

---

## SEARCH EXPANSION LADDER

When accepted items are fewer than 10, expand in this exact order, stopping once 10 are accepted:

1. official launches, releases, and announcements
2. GitHub same-day releases and unusual same-day attention spikes
3. Hacker News and Reddit same-day discussion spikes
4. Product Hunt launches
5. arXiv and Hugging Face Papers releases or notable publication spikes
6. company blogs and official newsroom posts
7. YouTube, Steam, or other same-day public signals allowed by this skill

Do not stop after checking only one or two ecosystems.

---

## STOPPING CONDITION

Stop searching when either:

- accepted items reach 10, or
- every ladder step (1–7) was searched with today's date filter

If the search stops with fewer than 10 accepted items, return them with shortfall_reason set (see SHORTFALL RULE).
Do not lower evidence standards, add speculative items, or pad to reach 10.

---

## SHORTFALL RULE

When fewer than 10 items are returned, set shortfall_reason to one short factual sentence that states:

- how many same-day candidates were checked
- the main reasons candidates were rejected (for example: missing date, unverifiable source, no same-day trigger, insufficient evidence for category or summary)

When 10 items are returned, shortfall_reason is null.
Describe only what was actually searched and rejected. Do not invent a cause.

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
- explicit publication date (YYYY-MM-DD), taken from the timestamp the source itself shows (GitHub release date, Hacker News or Reddit post time, Product Hunt launch date, arXiv submission date)
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
- Low: 1 ecosystem (rank below Medium and High items)

Low confidence is allowed for lower-ranked items, but only if the item still fully satisfies the date and evidence rules. Never accept a weaker item to reach 10.

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

Return ONLY a JSON object with these keys, in this order:

1. items: array of trend objects in ranked order (empty array if none qualify)
2. shortfall_reason: null when items has 10 entries; otherwise one factual sentence (see SHORTFALL RULE)

Preferred transport format: JSON.

This JSON-like structure is an internal data handoff for downstream rendering.
It is NOT the final user-facing presentation format.

If no qualifying items exist:
- return items as an empty array
- set shortfall_reason
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

---

## HANDOFF RULE FOR FINAL RENDERING

This skill is only the data layer.

If the caller intends to show user-facing content:
- it MUST pass the normalized records into the downstream presentation layer
- it MUST read and follow `references/output.md` for the final rendering contract
- it MUST NOT let this data-layer skill improvise Markdown directly

If the caller cannot guarantee the downstream renderer:
- return the structured data only
- do not mix JSON and Markdown
- do not generate a report-style summary as a fallback

The final renderer is responsible for:
- language
- headings
- line breaks
- bullet formatting
- section order
- failure copy

This separation is strict.
