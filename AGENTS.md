# Repository Guidelines

## Project Structure & Module Organization

This repository contains the OfferPilot Codex Skill. Keep the repository root minimal:

- `SKILL.md`: primary routing, workflow, safety, and interaction instructions.
- `agents/openai.yaml`: user-facing Skill metadata and default prompt.
- `references/source-evaluation.md`: source-quality and evidence rules loaded for online investigations.
- `references/report-templates.md`: concise output templates for job-risk, matching, and interview reports.

Add reusable guidance under `references/` and link it directly from `SKILL.md`. Keep `README.md` user-facing; do not duplicate detailed agent instructions there. Do not add changelogs or empty `scripts/` and `assets/` directories.

## Build, Test, and Development Commands

There is no compilation step or application runtime. Validate changes with:

```bash
python3 /Users/a1-6/.codex/skills/.system/skill-creator/scripts/quick_validate.py .
```

This checks frontmatter, required fields, and Skill naming rules. Before submitting, also check for unfinished template content:

```bash
rg -n 'TODO|\[TODO|Structuring This Skill' SKILL.md agents references
```

Review all files with `rg --files`; every committed file must directly support Skill execution.

## Coding Style & Naming Conventions

Write Markdown instructions in concise imperative language. Use ATX headings (`## Section`) and blank lines around lists. Keep `SKILL.md` under 500 lines and move detailed rules into one-level reference files.

Use lowercase kebab-case for Skill and directory names, for example `offerpilot` or `source-evaluation.md`. Keep YAML keys unquoted and quote all string values in `agents/openai.yaml`. Frontmatter may contain only `name` and `description`.

## Testing Guidelines

Run `quick_validate.py` after every metadata or structural change. Manually test representative prompts: a company name only, an inaccessible job link, a JD plus minimal background, and an interview-review request. Confirm the Skill gives partial value before asking questions, labels uncertainty, cites current sources, and never invents employment facts.

## Commit & Pull Request Guidelines

No Git history is currently available, so no existing convention can be inferred. Use short imperative commits such as `Add salary evidence rules` or `Fix benefit source labels`.

Pull requests should explain the user-visible behavior change, list modified workflows or references, include validation output, and provide a brief before/after example for prompt-behavior changes. Link related issues when available; screenshots are needed only for UI metadata changes.

## Security & Evidence Integrity

Never add real resumes, personal identifiers, confidential project metrics, or copied employee profiles as fixtures. Preserve the distinction between verified facts, reporting, public user feedback, inference, and unknown information. Negative claims must not be stronger than their supporting evidence.
