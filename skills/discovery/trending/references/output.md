# Output Format Specification

---

## Purpose

This file defines the FINAL presentation layer for the `trending` skill.

Input = normalized trend objects from the data layer.
Output = deterministic Markdown in Simplified Chinese.

Do not invent facts that are missing from the data layer.
If a required field is missing, skip that item instead of improvising.

---

## Default Format

- Output: Markdown
- Language: Simplified Chinese
- Tone: concise, factual, no hype
- Only use plain text if explicitly requested

---

## Input Contract Assumption

Each trend item is expected to provide:

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

If any of the following are missing, drop the item:

- name
- category
- summary
- why_it_matters
- suitable_audience
- trend_status
- expected_duration
- official_url
- evidence_signals

---

## Language Rule (STRICT)

- All explanatory text must be Simplified Chinese
- Keep project / product / paper names in their original official form
- Keep URLs unchanged
- Keep platform names unchanged when they are proper nouns, such as GitHub, Hacker News, Reddit, Product Hunt, arXiv, Hugging Face

Do not write English explanatory sentences.

---

## Title Rule

Item title must be the raw `name` value only.

Allowed:
- GPT-5
- LangChain
- Kubernetes
- 通义千问

Forbidden:
- GPT-5 趋势观察
- LangChain 框架发展趋势
- Kubernetes 今日热度分析

No subtitle.
No mixed commentary in the title.

---

## Fixed Field Order

Render every item in this exact order:

1. 标题（`## <name>`）
2. 名称
3. 分类
4. 摘要
5. 重要性
6. 适合人群
7. 趋势状态
8. 预计持续时间
9. 置信度
10. 证据信号
11. 官方链接

---

## Fixed Label Template

Use this exact label set for every item:

## <Name>

**名称**
<value>

**分类**
<value>

**摘要**
<value>

**重要性**
<value>

**适合人群**
<value>

**趋势状态**
<value>

**预计持续时间**
<value>

**置信度**
<value>

**证据信号**
- <value>

**官方链接**
<url>

---

## Value Rendering Rules

### 名称
- render `name` as-is

### 分类
- render the normalized category value as-is

### 摘要
- render `summary` as one sentence

### 重要性
- render `why_it_matters` as one sentence

### 适合人群
- if the input is an array, join with `、`
- if the input is already a string, keep it unchanged unless trivial whitespace cleanup is needed

### 趋势状态
Map enum values exactly as follows:

- Emerging → 新兴
- Growing → 增长中
- Mainstream → 主流
- Declining → 降温中

### 预计持续时间
Map enum values exactly as follows:

- Flash → 闪现型（几天）
- Short → 短期（1–4 周）
- Medium → 中期（1–3 个月）
- Long → 长期（3 个月以上）

### 置信度
Map values exactly as follows:

- High → 高
- Medium → 中
- Low → 低

### 证据信号
- Render 1–3 bullet points
- Preserve the source order from the data layer
- Each bullet must use this exact format:
	- `<source>｜<date>｜<signal>`
- Do not append commentary after the bullet
- Do not replace platform proper nouns with localized alternatives

### 官方链接
- render `official_url` only
- one line only

---

## Global Consistency Rules

- max 10 items
- no intro text
- no summary paragraph before the first item
- no conclusion
- no extra section after the last item
- one blank line between sections inside an item
- exactly one blank line between items
- do not renumber items
- do not add emojis
- do not add ranking badges unless ranking is explicitly provided upstream

---

## Anti-Localization Rule

- Do NOT convert global trends into Chinese internet equivalents
- Keep original ecosystem context such as GitHub / Hacker News / Reddit / Product Hunt / arXiv
- Output language is Chinese, but source ecosystem names remain global proper nouns

---

## Failure Rule

If no valid items remain after filtering, output exactly:

今天没有发现符合条件的趋势。