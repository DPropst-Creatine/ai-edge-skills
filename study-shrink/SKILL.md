---
name: study-shrink
description: Compresses scientific studies into token-efficient JSON or Markdown evidence cards while preserving methods, key results, limitations, and overstatement cautions.
---

# Study Shrink

## Purpose

Turn a scientific paper, abstract, PDF text, or pasted study excerpt into a compact, reusable evidence card for later LLM use.

Use this skill when the user wants to:
- reduce study text for token limits
- convert a study into JSON
- convert a study into Markdown
- extract methods/results/limitations
- prepare a paper for NotebookLM, ChatGPT, Gemini, Claude, or a research database
- create a citation-safe evidence card

## Core Rules

- Do not fabricate citation details, sample sizes, results, statistics, DOI, PMID, journal, or author names.
- Use `not reported` when information is missing.
- Preserve exact numerical results when provided.
- Distinguish direct paper findings from interpretation.
- Do not overstate causality unless the study design supports it.
- Do not rewrite a study title if the exact title is available.
- Do not invent mechanism, clinical relevance, or practice implications.
- Prefer concise structured extraction over narrative summary.
- If the input is only an abstract, clearly mark `source_depth: abstract_only`.
- If the input is full text or near-full text, mark `source_depth: full_text_or_near_full_text`.
- If the pasted text is too long or incomplete, ask for chunked input and produce a chunk digest.

## Default Behavior

If the user does not specify format, output both:

1. Compact JSON evidence card
2. Short Markdown brief

Target total output: 900–1,500 words unless the user asks for more.

## Compression Modes

The user may request one of these modes.

### `mode: tiny`

Use for very low token budgets.
Target: 250–500 words.
Output only:
- one-paragraph summary
- 5 key numbers
- 3 limitations
- what not to overstate

### `mode: json`

Output only the compact JSON evidence card.

### `mode: md`

Output only the Markdown brief.

### `mode: claim-map`

Output a table for content/research triage:
- Claim
- Evidence from paper
- Support level
- Caveat
- Can this be used in content?

### `mode: methods-results`

Focus only on study design, population, exposure/intervention, comparator, outcomes, statistical model, and results.

### `mode: chunk`

Use when the paper is too long. For each pasted chunk, output a compact chunk digest and wait for the next chunk. Do not produce final synthesis until the user says `final synthesis`.

## Compact JSON Evidence Card Schema

Use this schema. Keep fields short. Use arrays of short strings.

```json
{
  "source_depth": "abstract_only | full_text_or_near_full_text | excerpt_only | unknown",
  "citation": {
    "title": "",
    "first_author": "",
    "year": "",
    "journal": "",
    "doi": "",
    "pmid": ""
  },
  "study_type": "RCT | cohort | cross-sectional | case-control | systematic review | meta-analysis | narrative review | mechanistic | animal | protocol | editorial | other | not reported",
  "research_question": "",
  "population": {
    "n": "",
    "age": "",
    "sex": "",
    "setting": "",
    "inclusion_exclusion": ""
  },
  "exposure_or_intervention": "",
  "comparator": "",
  "duration_followup": "",
  "outcomes": [
    {
      "outcome": "",
      "result": "",
      "statistics": "",
      "direction": "benefit | harm | null | mixed | not reported"
    }
  ],
  "key_numbers": [],
  "authors_conclusion": "",
  "practical_interpretation": "",
  "limitations": [],
  "risk_of_bias_flags": [],
  "generalizability": "",
  "what_not_to_overstate": [],
  "content_angle": "",
  "one_sentence_takeaway": ""
}
```

## Markdown Brief Template

```markdown
# Study Evidence Card

## Citation
- Title:
- First author/year:
- Journal:
- DOI/PMID:
- Source depth:

## Question
[What the study tested]

## Design
[Study type, sample, setting, follow-up]

## Population
[Who was studied]

## Intervention/Exposure and Comparator
[What was compared]

## Key Results
- [Outcome]: [result + exact statistic if available]
- [Outcome]: [result + exact statistic if available]

## Interpretation
[Plain-language meaning without overstatement]

## Limitations / Risk of Bias
- [limitation]
- [limitation]
- [limitation]

## What Not to Overstate
- [caution]
- [caution]

## Reusable Takeaway
[1–2 sentences suitable for notes or future writing]
```

## Chunk Mode Template

When the user says `mode: chunk` or the input is too long, output:

```markdown
## Chunk Digest [#]
- Section detected:
- Key methods details:
- Key results/statistics:
- Definitions/measures:
- Limitations/signals:
- Citation details found:
- Keep for final synthesis:
- Missing/needs next chunk:
```

Then say: `Paste the next chunk, or say final synthesis.`

## Claim Map Template

```markdown
| Claim | Evidence from paper | Support level | Caveat | Use in content? |
|---|---|---|---|---|
|  |  | Strong / Moderate / Weak / Not supported |  | Yes / With caution / No |
```

## Token-Reduction Rules

When compressing for later LLM use:

- Keep exact sample size, intervention dose, follow-up duration, and primary outcomes.
- Keep exact effect sizes, confidence intervals, and p-values when available.
- Remove background paragraphs unless needed to interpret the study.
- Remove repeated author discussion unless it changes interpretation.
- Remove long mechanistic speculation unless directly tested.
- Replace long prose with short structured bullets.
- Keep definitions of exposures/outcomes.
- Keep statistical model/covariates if available.
- Keep funding/conflicts if relevant or concerning.

## Red Flags to Capture

Always note if present:

- small sample size
- short duration
- surrogate outcomes only
- post hoc or exploratory analysis
- no control group
- high attrition
- self-reported exposure/outcome
- industry funding or conflicts
- multiple comparisons
- unadjusted confounding
- non-generalizable population
- animal/in vitro evidence used for human claims

## Output Safety

For clinical or health interpretation:

- Do not state that a finding proves clinical benefit unless the study directly measured clinical outcomes.
- Do not convert preliminary or mechanistic evidence into patient recommendations.
- Do not imply standard of care from one study.
- State when findings are hypothesis-generating.

## User Commands

Recognize these commands:

- `json only`
- `md only`
- `tiny`
- `claim map`
- `methods/results only`
- `chunk mode`
- `final synthesis`
- `content angle`
- `clinical relevance`
- `what can I say publicly?`

## Final Check

Before answering, verify:

- Did I mark missing information as `not reported`?
- Did I preserve exact numbers that were provided?
- Did I avoid adding unsupported clinical claims?
- Did I include what not to overstate?
- Is the output much shorter than the original?
