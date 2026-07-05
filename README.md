# Prompt Forge

Prompt Forge is a compact prompt-architecture skill package.

It turns rough ideas, role descriptions, workflows, or existing drafts into
copyable prompt artifacts with model-aware structure, output contracts,
negative constraints, few-shot guidance, and lightweight hardening checks.

## What This Is

Use this package when the main deliverable is the prompt itself:

- system prompts,
- agent prompts,
- reviewer prompts,
- generator prompts,
- Gemini XML prompts,
- GPT or Claude prompts,
- prompt refactors,
- prompt hardening and comparison.

## What This Is Not

- It is not a general requirements-writing tool.
- It is not a task planner.
- It is not a decision stress-test skill.
- It is not a runtime application.
- It does not call an LLM by itself.

## Status

Active compact skill package on branch `main`.

This repo is the smaller successor to the older `prompt` reference repository.
It keeps the prompt architecture workflow while reducing the surface to the
files needed by an agent skill package.

## Relationship To `prompt`

```text
prompt
  older reference source with longer explanations and attack-vector material

prompt-forge
  current compact package for practical prompt creation and optimization
```

Use `prompt` when you need the long-form historical references. Use
`prompt-forge` when you need the installable prompt-building skill surface.

## Repository Layout

```text
.
  README.md
  SKILL.md
  agents/openai.yaml
  references/
    checklist.md
    model-dialects.md
    templates.md
```

## Trigger Boundary

Prompt Forge should trigger for requests like:

```text
write a prompt
optimize this prompt
make a system prompt
design an AI expert
turn this requirement into a prompt
generate a Gemini XML prompt
help me build a prompt generator
```

It should not trigger when the user wants source code, a product spec, a task
execution plan, or a decision analysis unless the final artifact is explicitly a
prompt.

## Usage

Clone the package:

```bash
git clone https://github.com/jupiternaut/prompt-forge.git
cd prompt-forge
```

Read the skill entrypoint first:

```bash
sed -n '1,220p' SKILL.md
```

Read references only when the task needs them:

```bash
sed -n '1,180p' references/templates.md
sed -n '1,180p' references/model-dialects.md
sed -n '1,180p' references/checklist.md
```

If installing manually into an agent skill directory, keep this structure
together:

```text
prompt-forge/
  SKILL.md
  agents/openai.yaml
  references/
```

## Tests And Checks

There is no automated test runner. Use these lightweight repository checks:

```bash
test -f SKILL.md
test -f agents/openai.yaml
test -f references/templates.md
test -f references/model-dialects.md
test -f references/checklist.md
```

Before treating a generated prompt as done, run the checklist in
`references/checklist.md`.

## Maintainer Notes

- Keep `SKILL.md` focused on prompt delivery.
- Keep model-specific details in `references/model-dialects.md`.
- Keep reusable shapes in `references/templates.md`.
- Do not let this repo drift into general task planning or requirements
  refinement.

## Related Repositories

- `prompt`: older long-form reference package.
- `Claude-skills`: adversarial analysis and browser-ops skill pack.

## Maintainer

jupiternaut / Geng Ruofan
