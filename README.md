# Better Prompt

`better-prompt` is a Codex skill for polishing prompts into clearer, more executable task instructions.

It is designed for prompts that target coding agents, general assistants, creative models, reviewers, or workflow agents. The skill keeps ordinary prompt rewrites concise, while loading deeper rubric and iteration guidance only for complex tasks.

## What It Does

- Clarifies the real task, context, constraints, success criteria, and output format.
- Removes prompt bloat, repeated rules, vague wording, and hidden assumptions.
- Chooses the smallest sufficient prompt strength: short task, closed-loop goal, or complex requirement.
- Adds verification or review criteria when the downstream agent can check its work.
- Provides reusable templates for agent prompts and skill-building prompts.

## Install

Clone this repository into your Codex skills directory:

```bash
git clone https://github.com/JinUnique/better-prompt.git "${CODEX_HOME:-$HOME/.codex}/skills/better-prompt"
```

Then restart or refresh Codex so the skill can be discovered.

## Usage

Ask Codex to use the skill when rewriting a prompt:

```text
Use $better-prompt to polish this prompt:
{your draft prompt}
```

Common trigger phrases include:

- `润色提示词`
- `优化提示词`
- `改写提示词`
- `prompt polish`
- `improve this prompt`

## Structure

```text
better-prompt/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── prompt-optimization.md
```

- `SKILL.md`: core workflow, trigger behavior, and output contract.
- `agents/openai.yaml`: UI metadata for Codex.
- `references/prompt-optimization.md`: deeper rubric, templates, and optional metric-driven iteration guidance.

## Validation

Validate the skill with the Codex skill creator's `quick_validate.py` script:

```bash
python path/to/quick_validate.py path/to/better-prompt
```

Expected result:

```text
Skill is valid!
```
