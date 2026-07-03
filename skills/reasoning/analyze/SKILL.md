---
name: analyze
description: Deep analysis, evaluation, critique, and explanation of provided content (text, URLs, files, logs, code, configs). Focus on reasoning, trade-offs, assumptions, and implications rather than summarization.
compatibility: Requires optional outbound HTTP access for URL expansion (fetch/requests/urllib). No additional dependencies required.
---

# ANALYZE (DATA + REASONING LAYER ONLY)

## Purpose

Transform raw input into structured understanding and actionable insight.

The goal is to answer:

- What is this?
- Why does it exist?
- How does it work?
- What assumptions does it make?
- What are its trade-offs?
- What are its risks and limitations?
- What should be done about it?

This layer prioritizes:
- reasoning
- evaluation
- critique
- decomposition

NOT:
- formatting
- presentation style
- language rules
- output structure

---

## Supported Inputs

Accept:

- plain text
- URLs
- documents
- articles
- code
- logs
- configs
- repositories
- mixed structured data

Multiple inputs may be combined.

---

## SOURCE POLICY (GLOBAL + PRIMARY ONLY)

When external context is required:

- Prefer official documentation
- Prefer primary sources
- Prefer globally accessible sources
- Avoid single-source dependency
- Use multiple independent references when possible

If a source is unavailable:
→ switch to equivalent accessible global source
→ do NOT reduce analysis quality

---

## ANALYSIS WORKFLOW

### 1. Identify Target

Determine:

- system / project / concept / issue type
- domain (tech / product / security / research / etc.)

If ambiguous:
→ explicitly state assumptions

---

### 2. Context Acquisition

If URL provided:
- extract relevant sections only
- prioritize primary documentation or source repo
- ignore irrelevant pages

If structured input:
- treat as authoritative
- do not reformat unless necessary for reasoning

---

### 3. Reasoning Phase

Always analyze through:

- design intent
- system behavior
- architecture or structure
- dependencies
- assumptions
- constraints
- trade-offs
- failure modes

Focus on WHY, not just WHAT.

---

### 4. Evaluation Phase

Depending on task type:

#### Analysis
- explain structure
- clarify mechanism
- interpret behavior

#### Critique / Review
- identify good design choices
- identify weaknesses
- expose hidden risks
- evaluate consequences

#### Comparison
- contrast trade-offs
- highlight suitability differences
- identify best-fit scenarios

#### Debug / Log analysis
- isolate root cause
- identify failure chain
- suggest fix direction

---

## OUTPUT CONTRACT (DATA ONLY)

Return structured reasoning objects:

- summary
- key_points
- assumptions
- strengths
- weaknesses
- risks
- recommendations
- confidence

NO formatting instructions.
NO Markdown.
NO presentation rules.
NO language rules.

---

## CONFIDENCE RULE

Each analysis must include:

- high / medium / low confidence

Based on:
- completeness of input
- availability of sources
- ambiguity level

---

## QUALITY RULES

- Prefer insight over description
- Prefer mechanism over summary
- Prefer critique over repetition
- Avoid generic statements
- Avoid unsupported praise
- Explicitly state uncertainty when present
- Do not fabricate missing context

---

## FAILURE MODE

If input is insufficient:

- explicitly state missing information
- proceed with best-effort reasoning
- do not hallucinate facts

---

## OUTPUT SEPARATION RULE

This skill MUST NOT:

- define language
- define Markdown
- define formatting
- define final structure

It ONLY produces structured analytical content for downstream formatting layer.
