---
name: skill-forge
description: Creates AI Edge Gallery skill folders by drafting SKILL.md files and optional JavaScript helper scaffolds with scripts/index.html, assets notes, test prompts, and setup instructions.
---

# Skill Forge

## Purpose

Create practical Google AI Edge Gallery skill files from a short idea.

Use this skill when the user wants to:
- make a new skill
- improve an existing skill
- create a SKILL.md file
- scaffold a JavaScript skill
- prepare files for a skill folder
- turn a workflow into a reusable mobile AI skill

## Important Limitations

- This skill generates copy-paste-ready files. It does not automatically create files on GitHub or on the phone.
- For text-only skills, the main output is `SKILL.md`.
- For JavaScript skills, output a folder tree plus `SKILL.md` and `scripts/index.html`.
- Do not claim that companion files are read by the model unless JavaScript is used to load or execute them.
- Keep mobile model stability in mind: shorter, stricter skills usually work better.

## Skill Types

### Text-only skill

Use for:
- writing workflows
- checklists
- structured outputs
- coaching prompts
- clinical or research triage
- content templates

Folder:

```text
skill-name/
└── SKILL.md
```

### JavaScript helper skill

Use for:
- calculators
- databases
- parsers
- scoring rules
- deterministic formatting
- persistent localStorage memory

Folder:

```text
skill-name/
├── SKILL.md
└── scripts/
    └── index.html
```

### Interactive webview skill

Use cautiously. More moving parts can destabilize small mobile models.

Folder:

```text
skill-name/
├── SKILL.md
├── scripts/
│   └── index.html
└── assets/
    └── dashboard.html
```

## Generator Defaults

When details are missing, make reasonable assumptions and generate a useful v1.

Default output should include:
1. Folder tree
2. `SKILL.md`
3. Optional `scripts/index.html` when JavaScript is useful
4. Test prompts
5. Notes about limitations and next improvements

## When To Use JavaScript

Recommend JavaScript only when the workflow benefits from deterministic computation or structured data.

Use JavaScript for:
- database lookup
- scoring formulas
- calculators
- parsers
- trackers
- repeatable JSON/plain-text transforms

Avoid JavaScript for:
- simple writing templates
- prompts that only need instructions
- tasks where the LLM must primarily reason over text

## JavaScript Contract

For JS skills, `scripts/index.html` should expose:

```javascript
window.ai_edge_gallery_get_result = async function(dataStr) {
  const input = JSON.parse(dataStr || '{}');
  return JSON.stringify({ result: 'plain text result here' });
}
```

The tool should return a concise `result` string the LLM can copy into the answer.

## Required Output Style

When asked to create a skill, respond with:

```text
Skill name: [name]
Skill type: [text-only / JavaScript helper / interactive webview]
Why this type: [brief reason]

Folder tree:
[tree]

File: SKILL.md
```markdown
[full SKILL.md]
```

[If JS skill]
File: scripts/index.html
```html
[full index.html]
```

Test prompts:
1. [prompt]
2. [prompt]
3. [prompt]

Implementation notes:
- [brief]
```

## Quality Rules

- Use kebab-case folder names.
- Keep `SKILL.md` focused and not overly long.
- Give strict output templates for unstable mobile models.
- Include examples.
- Include failure handling.
- Include a short test plan.
- Avoid fragile behavior like large webviews unless needed.
- Do not fabricate claims about APIs or app capabilities.
- Use plain text outputs unless the user explicitly asks for JSON.

## Optional JavaScript Generator

When the user gives a structured request, call `run_js` with script `index.html` and JSON like:

```json
{
  "action":"create_skill",
  "skill_name":"spc-food-lookup",
  "skill_type":"javascript",
  "description":"Look up foods and return SPC tracker blocks",
  "purpose":"Create database-based SPC estimates",
  "inputs":["food description", "meal type"],
  "outputs":["paste-ready tracker block"],
  "rules":["Do not output JSON to user", "Use SPC language"],
  "examples":["Build a meal from half turkey sandwich and soup"]
}
```

If the tool returns a scaffold, present the files cleanly to the user.
