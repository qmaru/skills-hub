# Output Format Specification

---

## Default Format

- Output: Markdown
- Only use plain text if explicitly requested

---

## Language Rule (STRICT OUTPUT LAYER ONLY)

- Output MUST be Simplified Chinese
- NO English sentences in explanations

Allowed English only:
- project / product names
- URLs
- technical terms without standard Chinese translation

---

## Trend Name Rule

Allowed:
- GPT-5
- LangChain
- Kubernetes

Forbidden:
- LangChain 框架发展趋势
- GPT-5 model analysis report

Rule:
- must be either pure English OR pure Chinese
- no mixed-language titles

---

## Required Structure (fixed order)

1. Name
2. Category
3. Summary
4. Why it matters
5. Suitable audience
6. Trend status
7. Expected duration
8. Evidence signals
9. Official URL

---

## Markdown Template

## <Name>

**Name**
<value>

**Category**
<value>

**Summary**
<one sentence>

**Why it matters**
<value>

**Suitable audience**
<value>

**Trend status**
<Emerging | Growing | Mainstream | Declining>

**Expected duration**
<value>

**Evidence signals**
- <source + date>
- <source + date>

**Official URL**
<url>

---

## Constraints

- max 10 items
- no intro text
- no conclusion
- strict field order
- one blank line between items
- no extra commentary

---

## Anti-Localization Rule

- DO NOT convert global trends into Chinese internet equivalents
- Keep original ecosystem context (GitHub / HN / Reddit / PH)
- Output language ≠ data language

---

## Failure Rule

If no valid items:

"No qualifying trends were found today."