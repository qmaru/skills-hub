# Output

The chart image MUST be produced by the Flint MCP tool `render_chart`.

`render_chart` returns an image artifact (PNG or SVG). It does NOT return a Data URL.

The Data URL is the final processing step, performed after `render_chart`:

1. Take the image bytes returned by `render_chart`.
2. Base64-encode those bytes. The encoded output must not contain line breaks.
3. Construct the Data URL:

   data:image/png;base64,<BASE64>

   (or `data:image/svg+xml;base64,<BASE64>` when SVG was requested.)

Do NOT render or screenshot the chart manually. Only the final base64 encoding of the `render_chart` output is performed by you.

## Hard Output Contract

- The Flint specification, Vega-Lite, ECharts, Chart.js, and any other spec text are **internal only**. They are passed to `render_chart` as tool input and are **never emitted** to the user.
- The response MUST always be a stable PNG Data URL. Default to PNG (`data:image/png;base64,...`) unless the user explicitly asks for SVG.
- Never output JSON of any kind as the response.

## Anti-Fallback Rules (CRITICAL)

The image IS the deliverable. There is no fallback path that substitutes analysis or prose for the chart.

- The Data URL is the image itself — return it directly. Do not claim the image cannot be shown or that a text substitute is needed.
- Do not invent or assume data when the real input is missing. Fabricated values, placeholder datasets, or "for example" datasets are never acceptable.
- Do not drift into analysis mode: no tables, findings, summaries, or explanatory prose standing in for the chart.
- Do not explain, annotate, or apologize in the response. The only valid response is the Data URL line.
- This skill only renders; it does not analyze, summarize, or interpret data. Any such work belongs to a separate upstream step (e.g. `reasoning/analyze`), not here.
- If the required data is genuinely missing and cannot be obtained, STOP and ask the user for it — do not invent it and do not emit analysis.

## Final Output Rules

The final response MUST be exactly one line.

The final response MUST start with:

data:image/png;base64,

(or `data:image/svg+xml;base64,...` for SVG output.)

The final response MUST contain nothing except the Data URL.

Do NOT wrap the output in Markdown.

Do NOT use code fences.

Do NOT output triple backticks (```).

Do NOT output inline code.

Do NOT output JSON.

Do NOT output the Flint specification or any chart spec.

Do NOT output XML.

Do NOT output HTML.

Do NOT output explanations.

Do NOT output notes.

Do NOT output labels.

Do NOT output headings.

Do NOT output blank lines.

Return only the Data URL.