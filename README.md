# Better Prompt

`better-prompt` is a Codex skill for turning complex or high-quality task requirements into executable prompts for agents and workflow systems.

It is not a general prose editor. Do not use it for ordinary wording polish, one-sentence rewrites, simple translation, casual questions, or lightweight prompt polish. Use it when the request needs clear goals, constraints, agent coordination, success criteria, verification, failure handling, and a delivery format.

## What It Does

- Converts complex requirements into agent-executable task prompts.
- Defines goals, key results, context, constraints, acceptance criteria, verification, and stop conditions.
- Defaults to a supervised execution loop: research, plan, execute, validate, summarize.
- Uses a main-agent and subagent structure when the target environment supports subagents.
- Provides a single-agent staged fallback when subagents are unavailable.
- Removes repeated rules, vague quality language, unsupported capabilities, and process bloat.

## Install

Clone this repository into your Codex skills directory:

```bash
git clone https://github.com/JinUnique/better-prompt.git "${CODEX_HOME:-$HOME/.codex}/skills/better-prompt"
```

Then restart or refresh Codex so the skill can be discovered.

## Development Source

The WSL development source should live at:

```bash
/root/projects/better-prompt
```

Runtime installation copies may exist at:

```bash
/root/.hermes/skills/better-prompt
C:\Users\陈锦涛\.codex\skills\better-prompt
```

Treat those runtime locations as sync targets, not the source of truth.

## Usage

Ask Codex to use the skill when rewriting a complex task prompt:

```text
Use $better-prompt to optimize this complex task prompt:
{your draft requirement}
```

Common trigger phrases include:

- `优化任务提示词`
- `改写成高质量执行提示词`
- `把这个需求整理成 agent 可执行提示词`
- `设计多智能体执行提示词`
- `optimize this complex task prompt`
- `rewrite this into an executable agent prompt`

## Structure

```text
better-prompt/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── prompt-optimization.md
```

- `SKILL.md`: trigger boundary, core workflow, default structure, and output contract.
- `agents/openai.yaml`: Codex UI metadata.
- `references/prompt-optimization.md`: reusable templates, rubric, orchestration guidance, fallback wording, and quality checks.

## Validation

Validate the skill with the Codex skill creator's `quick_validate.py` script:

```bash
python /mnt/c/Users/陈锦涛/.codex/skills/.system/skill-creator/scripts/quick_validate.py /root/projects/better-prompt
```

Expected result:

```text
Skill is valid!
```
