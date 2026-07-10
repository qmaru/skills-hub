# Output

Render the chart as a PNG file.

Encode the PNG using the operating system's `base64` command.

The encoded output must not contain line breaks.

Construct the final Data URL using this format:

data:image/png;base64,<BASE64>

## Final Output Rules

The final response MUST be exactly one line.

The final response MUST start with:

data:image/png;base64,

The final response MUST contain nothing except the Data URL.

Do NOT wrap the output in Markdown.

Do NOT use code fences.

Do NOT output triple backticks (```).

Do NOT output inline code.

Do NOT output JSON.

Do NOT output XML.

Do NOT output HTML.

Do NOT output explanations.

Do NOT output notes.

Do NOT output labels.

Do NOT output headings.

Do NOT output blank lines.

Use only the operating system's `base64` command.

Return only the Data URL.