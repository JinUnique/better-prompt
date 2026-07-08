# Better Prompt

`better-prompt` is a Codex skill for turning complex or high-stakes requirements into high-delivery `/goal` task prompts.

It is not a general prose editor. Do not use it for ordinary wording polish, one-sentence rewrites, simple translation, casual questions, or lightweight prompt polish. Use it when the request needs a durable objective, constraints, agent coordination, success criteria, verification, stop conditions, and a delivery format.

## What It Does

- Converts complex requirements into Codex `/goal` contracts.
- Designs compact long-running task goals: outcome, verification surface, constraints, boundaries, iteration policy, and blocked stop condition.
- Defines goals, key results, context, constraints, acceptance criteria, verification, and stop conditions.
- Defaults to a supervised execution loop: research, plan, execute, validate, summarize.
- Uses a main-agent and subagent structure for complex task execution.
- Removes repeated rules, vague quality language, unsupported capabilities, oversized background dumps, and process bloat.

## Install

Clone this repository into your agent skills directory:

```bash
git clone https://github.com/JinUnique/better-prompt.git /path/to/agent/skills/better-prompt
```

Then restart or refresh the target agent so the skill can be discovered.

## Development Source

The WSL development source should live at:

```bash
/root/projects/better-prompt
```

Runtime installation copies may exist at:

```bash
/root/.hermes/skills/better-prompt
```

Treat those runtime locations as sync targets, not the source of truth.

## Usage

Ask Codex to use the skill when drafting or rewriting a `/goal` prompt:

```text
Use $better-prompt to optimize this complex task prompt:
{your draft requirement}
```

Common trigger phrases include:

- `优化任务提示词`
- `改写成高质量执行提示词`
- `把这个需求整理成 agent 可执行提示词`
- `设计 /goal 提示词`
- `把这个需求整理成 Codex /goal`
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
- `agents/openai.yaml`: agent UI metadata.
- `references/prompt-optimization.md`: reusable templates, rubric, orchestration guidance, failure handling, and quality checks.

## Validation

Validate the skill with the skill creator's `quick_validate.py` script:

```bash
python /path/to/skill-creator/scripts/quick_validate.py /root/projects/better-prompt
```

Expected result:

```text
Skill is valid!
```
