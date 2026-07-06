# Output Format Specification

---

## Purpose

This file defines the FINAL presentation layer for the `trending` skill.

Input = normalized trend objects from the data layer.
Output = final deterministic Markdown in Simplified Chinese, ready to be rendered directly as webpage content.

Do not invent facts that are missing from the data layer.
If a required field is missing, skip that item instead of improvising.

---

## Default Format

- Output: Markdown
- Language: Simplified Chinese
- Tone: concise, factual, no hype
- Mobile-friendly: prefer headings + short paragraphs + bullet lists
- Never use tables
- The output itself is the final deliverable, not an explanation of the format
- Write directly as renderable Markdown content
- Use repeated card-like sections, not a digest list
- Never output raw JSON, YAML, XML, or other structured serialization formats
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

Even if the input arrives as JSON objects, arrays, or any other structured representation,
the final answer MUST be rendered as Markdown content only.

Never echo the source structure.
Never wrap the result in a code fence.

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
2. 元信息行（分类 / 趋势状态 / 预计持续时间 / 置信度）
3. 正文段落（摘要 + 重要性）
4. 适合人群行
5. 证据信号
6. 官方链接

Every item MUST begin with a level-2 Markdown heading: `## <name>`.
Do not use ordered lists as the outer structure.
Do not group multiple items under one shared heading.
Do not add a date banner, page title, or topic digest heading above the first item.

---

## Fixed Label Template

Use this exact label set for every item:

This section defines the exact final Markdown shape that should be emitted.
Do not describe the template in the answer.
Do not explain what will be rendered.
Directly output the rendered Markdown body.

Do not wrap the result with any meta text such as:
- 以下是结果
- 输出如下
- 渲染结果
- 输出至 xxx
- 结果已生成

The rendered output must start directly with the first trend item.

The first non-empty line of the entire output MUST be:
`## <name>`

Never start with text such as:
- Here are today's ...
- 今日热点如下
- 根据今日搜索结果
- 以下是 2026 年 7 月 5 日 AI 领域的热点话题
- 2026年7月4日 AI热点
- 🔥 今日 AI 热点
- 1.
- -
- [
- {
- ```json
- ```

## <Name>

<分类>｜<趋势状态>｜<预计持续时间>｜置信度：<置信度>

<摘要> <重要性>

适合人群：<value>

- <value>

<url>

---

## Value Rendering Rules

### 标题
- render `name` as-is

### 元信息行
- render on a single line immediately below the title
- use this exact format:
	`<category>｜<trend_status>｜<expected_duration>｜置信度：<confidence>`
- do not add a label such as “元信息”
- do not split this line into bullets
- do not merge this line into the title
- do not move this line below the evidence list

### 摘要
- merge `summary` and `why_it_matters` into one compact paragraph
- format: `<summary> <why_it_matters>`
- keep it to 2 sentences total
- do not add labels such as “摘要” or “重要性”
- do not turn the paragraph into a list item
- do not prefix it with dashes, numbers, or quotes

### 重要性
- do not render as a separate section
- append directly after `summary`

### 适合人群
- render as a single line using this exact prefix: `适合人群：`
- if the input is an array, join with `、`
- if the input is already a string, keep it unchanged unless trivial whitespace cleanup is needed
- do not convert this line into parentheses or trailing inline notes after the paragraph

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

After mapping, render only inside the metadata line.

### 证据信号
- Render 1–3 bullet points
- Preserve the source order from the data layer
- Each bullet must use this exact format:
	- `<source>｜<date>｜<signal>`
- do not add a section label such as “证据信号”
- Do not append commentary after the bullet
- Do not replace platform proper nouns with localized alternatives

### 官方链接
- render `official_url` only as the final line of the item
- do not add a section label such as “官方链接”
- one line only
- do not wrap the URL in extra prose such as “来源：” or “链接：”

---

## Global Consistency Rules

- max 10 items
- render all upstream items up to 10
- do not arbitrarily stop after 3, 5, or 6 items if more valid items are available upstream
- output content only
- final Markdown only
- never expose intermediate JSON
- never return a JSON array or object to the user
- never use fenced code blocks for the final answer
- no intro text
- no page title above the first item
- no H1 heading such as `# 今日 AI 热点` or `# 2026年7月4日 AI热点`
- no H3/H4 wrapper heading above an item block
- no summary paragraph before the first item
- no conclusion
- no extra section after the last item
- no wrapper text before or after the content
- no status text such as “输出至 xxx”, “已渲染”, “结果如下”
- no explanation of layout, structure, or formatting choices
- no tables
- no ordered-list outer structure
- no single numbered digest containing multiple trends
- no “标题 + 1..10 列表” layout
- keep each item visually compact
- avoid repeating information already present in the title
- use one compact paragraph instead of multiple explanatory subsections
- prefer final webpage copy over report-like field rendering
- one blank line between sections inside an item
- exactly one blank line between items
- do not renumber items
- do not add emojis
- do not add ranking badges unless ranking is explicitly provided upstream
- do not prefix item titles with `1.`, `2.`, or any other numbering
- do not render source bullets with labels like `来源：`, `信号：`, or `官方链接：`
- do not replace the metadata line with loose prose or separate badges
- do not insert decorative separators before the first item

---

## Disallowed Output Patterns

The following patterns are explicitly invalid even if the facts are correct:

### Invalid pattern A: numbered digest

Here are today's AI热点:

1. 项目名 — 一整句说明（来源：xxx）
2. 项目名 — 一整句说明（来源：xxx）

Why invalid:
- uses a shared intro heading
- uses ordered list as the outer structure
- does not render each item as an independent Markdown card

### Invalid pattern E: date banner + section heading

根据今日搜索结果，以下是 2026年7月5日 AI领域的热点话题：

---

## 今日 AI 热点

1. Anthropic 发布 Claude Sonnet 5

Why invalid:
- adds wrapper prose before the first item
- adds a shared section heading
- turns items into a numbered digest
- does not start directly with `## <name>`

### Invalid pattern F: date headline page layout

# 2026年7月4日 AI热点

1. Claude Fable 5 恢复全球可用

Why invalid:
- uses a page title instead of item cards
- uses ordered-list outer structure
- breaks the required first-line rule

### Invalid pattern G: semi-structured card with custom labels

## JadePuffer

Security｜新兴｜闪现型（几天）｜置信度：高

...

- BleepingComputer｜2026-07-04｜...

https://example.com

Why invalid when produced from this skill:
- this shape is only valid if every required field is present in the exact canonical order
- custom prefaces, extra prose wrappers, or omitted required lines make the card invalid
- the renderer must follow the canonical template exactly, not approximately

### Invalid pattern B: single-line compressed item

## 项目名 — 分类｜状态｜持续时间｜置信度｜摘要｜来源

Why invalid:
- merges multiple required sections into one line
- destroys mobile readability

### Invalid pattern C: report-style field dump

## 项目名
摘要：...
重要性：...
适合人群：...
官方链接：...

Why invalid:
- too report-like
- does not follow the canonical final webpage copy shape defined above

### Invalid pattern D: raw JSON output

[
	{
		"name": "OpenCode",
		"category": "Developer Tools"
	}
]

Why invalid:
- exposes internal structured data instead of final rendered Markdown
- not directly usable as webpage body content in this skill's output contract

---

## Example Output

Use the following shape as the canonical example of the final rendered Markdown:

If there is any uncertainty about spacing, line breaks, or section layout:
- prefer matching this example over inventing a new structure
- keep the same visual rhythm and section order
- do not introduce new labels, wrappers, or decorative elements
- if a generated answer visually resembles any invalid pattern above, rewrite it before returning

## OpenCode

Developer Tools｜增长中｜中期（1–3 个月）｜置信度：中

OpenCode 今日发布新版本，并在开发者社区内同步获得明显讨论热度，成为一个快速升温的 AI 编码工具项目。它反映出开发者对本地优先、可扩展、可自托管 AI 编码工作流的需求仍在持续增强。

适合人群：开发者、工具链团队、AI 应用构建者

- GitHub｜2026-07-03｜发布新版本并出现明显关注增长
- Hacker News｜2026-07-03｜进入当日讨论流并获得开发者反馈

https://github.com/example/opencode

## Nano Browser Agent

AI｜新兴｜短期（1–4 周）｜置信度：高

Nano Browser Agent 今日因新演示发布与社区扩散而快速升温，展示了浏览器代理在真实任务执行上的更成熟形态。它让浏览器代理从概念展示进一步走向可落地工具，可能影响自动化办公、测试和信息采集场景。

适合人群：开发者、研究者、自动化团队、效率工具用户

- 官方博客｜2026-07-03｜发布新演示并说明核心能力升级
- Reddit｜2026-07-03｜出现集中讨论与体验反馈
- Product Hunt｜2026-07-03｜当日上线并获得早期关注

https://example.com/nano-browser-agent

---

## Anti-Localization Rule

- Do NOT convert global trends into Chinese internet equivalents
- Keep original ecosystem context such as GitHub / Hacker News / Reddit / Product Hunt / arXiv
- Output language is Chinese, but source ecosystem names remain global proper nouns

---

## Failure Rule

If no valid items remain after filtering, output exactly:

今天没有发现符合条件的趋势。
