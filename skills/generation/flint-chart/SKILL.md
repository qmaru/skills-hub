---
name: flint-chart
description: |
  Expert workflow for creating charts with Microsoft Flint.

  Invoke this skill whenever the request involves:

  - charts
  - graphs
  - dashboards
  - visualization
  - plotting
  - data analysis
  - chart recommendation
  - Vega-Lite
  - ECharts
  - Chart.js
---

# Flint Chart Workflow

Reference

https://microsoft.github.io/flint-chart/#/
https://microsoft.github.io/flint-chart/#/documentation/agent-workflows

---

## MCP Tools (tool-preferred)

This skill is **tool-preferred**. Use the Flint MCP server instead of rendering or encoding by hand.

Available tools:

- `author_flint_chart` (prompt): load before authoring to get the latest `ChartAssemblyInput` authoring rules.
- `flint://agent-skill` (resource): bundled authoring instructions for valid specs.
- `flint://chart-types` (resource): browsable catalog of chart types and encoding channels.
- `list_chart_types`: list chart types / channels, optionally scoped to one backend.
- `compile_chart`: return backend-native spec (Vega-Lite / ECharts / Chart.js) + assembly warnings.
- `validate_chart`: validate a spec, return warnings/errors and computed chart size.
- `render_chart`: return a static PNG or SVG image artifact. **Use this to produce the chart image.**
- `create_chart_view`: open the interactive MCP App (live SVG preview). Use when the user wants to see or interact with the chart.

Hard rule: never render or screenshot the chart manually. Always obtain the image from `render_chart`. The Data URL is produced afterward by base64-encoding the `render_chart` image output.

**Output contract (non-negotiable):** the only thing ever returned to the user is a `data:image/png;base64,...` Data URL produced by `render_chart`. The Flint specification is an internal artifact passed to `render_chart` — it is never emitted, printed, or shown to the user. Never output JSON, Vega-Lite, ECharts, Chart.js, or any spec text as the response.

---

## Goal

Convert user intent into a valid Flint specification.

Never start by writing Vega-Lite, ECharts or Chart.js.

Always reason in Flint first.

# Scope — Render Only

This skill RENDERS charts. It does NOT analyze data.

- Data analysis, interpretation, and insight extraction are handled by a separate upstream step, not this skill.
- This skill takes the visualization intent (and any data already provided) and produces a chart image. It must never invent data, write analysis prose, or fall back to "here is the analysis instead" when it cannot show an image.
- The image (PNG Data URL) is the only output. See `references/output.md`.

---

# Workflow

Follow every step in order.

## Step 1 — Understand the request

Determine:

- visualization goal
- expected audience
- important measures
- important dimensions
- comparison or relationship
- whether the user already supplied data

If required information is missing,
ask only the minimum questions.

---

## Step 2 — Understand the dataset

Identify:

Dimensions

Measures

Temporal fields

Geographic fields

Identifiers

Ignore columns unrelated to visualization.

---

## Step 3 — Infer semantic types

Infer semantic types for every field.

Examples:

Category

Ordinal

Nominal

Currency

Percentage

Temperature

Boolean

Latitude

Longitude

Year

YearMonth

Date

Timestamp

Quantity

Rank

Never require the user to specify these unless ambiguous.

---

## Step 4 — Select chart type

Choose the chart that best answers the question.

Typical mapping:

Comparison
→ Bar

Trend
→ Line

Distribution
→ Histogram

Relationship
→ Scatter

Composition
→ Pie

Hierarchy
→ Treemap

Correlation
→ Scatter

Density
→ Heatmap

Ranking
→ Ordered Bar

If multiple charts are reasonable,
briefly explain why one was selected.

---

## Step 5 — Build encodings

Use Flint encodings.

Possible channels:

x

y

color

size

shape

theta

radius

column

row

tooltip

text

Only include necessary encodings.

---

## Step 6 — Produce Flint specification (internal only)

Build a complete Flint specification **in memory / as a tool input**. This spec is never shown to the user.

Before finalizing, call `validate_chart` on the spec to catch errors/warnings and confirm the computed size. Fix any reported issues.

Structure (internal — do NOT emit this as output):

```json
{
  "data": {},
  "semantic_types": {},
  "chart_spec": {
    "chartType": "",
    "encodings": {}
  }
}
```

The specification must be internally consistent.

Immediately pass this spec to `render_chart` to obtain the PNG image. Do not print the spec.

---

## Step 7 — Optional compilation

Only if explicitly requested.

Supported targets:

- Vega-Lite
- ECharts
- Chart.js

Use the `compile_chart` MCP tool to obtain the backend-native spec plus assembly warnings.

Compilation order:

Flint

↓

Target format (via `compile_chart`)

Never skip Flint.

---

# Validation Checklist

Before returning the result verify:

✓ chart type matches user intent

✓ semantic types are complete

✓ encodings are valid

✓ measures and dimensions are not reversed

✓ unnecessary channels removed

✓ specification is minimal

---

Internal reasoning is allowed, but the **final emitted response** must follow `references/output.md` exactly: a single Data URL line produced by `render_chart`.

When reasoning is shown (e.g. in an interactive session), keep it strictly to prose. Never print the Flint spec or any JSON.

## Reasoning

Brief explanation of chart selection (prose only — no code blocks, no JSON).

The final line returned to the caller MUST be the Data URL from `render_chart`, with no surrounding text.

---

# Rules

Never invent data.

Never fabricate field names.

Never omit semantic_types.

Never generate rendering-library configuration first.

Prefer Flint-native concepts over renderer-specific options.

Keep the specification concise.

---

# Reference

Official documentation:

https://microsoft.github.io/flint-chart/#/
https://microsoft.github.io/flint-chart/#/documentation/agent-workflows

## References

- references/output.md

The requirements in references/output.md are mandatory.

When producing the final response, follow references/output.md exactly.

If there is any conflict between this skill and references/output.md, references/output.md takes precedence.

Do not produce any output before applying the rules in references/output.md.
