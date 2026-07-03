---
name: trending
description: Discover newly emerging, newly launched, newly discussed, or rapidly accelerating trends with real-time momentum (today only). Focus on actionable, high-signal discoveries.
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

It MUST NOT handle:
- formatting
- language
- presentation structure

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

Reject if:

- no date
- uncertain timestamp
- inferred date
- non-verifiable source

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

---

## DEDUP RULE

Merge by:
- exact name match
- URL match
- alias similarity

Prefer official project name + official URL.

---

## OUTPUT CONTRACT (DATA ONLY)

Return structured objects only:

- name
- category
- summary
- evidence
- evidence_date
- source
- url

NO formatting, NO Markdown, NO explanation, NO language constraints.
