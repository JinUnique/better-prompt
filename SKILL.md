---
name: better-prompt
description: Polish, rewrite, and optimize prompts for stronger agent execution. Use when the user asks to "润色提示词", "优化提示词", "改写提示词", "润色：...", "prompt polish", "improve this prompt", or provides a draft prompt that should be made clearer, shorter, more executable, more verifiable, or better structured for an AI agent, coding agent, creative model, or workflow agent. Also use when converting vague requests into task prompts with goals, context, success criteria, verification, constraints, and final response format. Do not use for normal prose editing unless the text is intended to be a prompt.
---

# Better Prompt

## Core Rule

Optimize prompts for execution quality, not decoration. Prefer the shortest prompt that preserves the user's intent, removes ambiguity, gives the agent enough context, and includes observable success criteria or verification when the task allows it.

## Workflow

1. Identify the target executor: coding agent, general assistant, image model, video model, music model, reviewer, or other workflow agent.
2. Choose the smallest sufficient prompt strength: short task, closed-loop goal, or complex requirement. Infer this automatically; ask only when the draft is ambiguous and the choice would materially change the output.
3. Extract the user's real objective, required context, hard constraints, output format, and any verification signal from the draft.
4. Remove prompt bloat: repeated rules, vague praise words, hidden assumptions, excessive process narration, and generic "be professional" language.
5. Rewrite into an execution-first structure:
   - Task
   - Context
   - Requirements
   - Success criteria
   - Verification or review method
   - Output format
6. Keep optional blocks optional. Add research, independent review, evaluation loops, examples, or generalization rules only when they materially improve the task outcome. Add generalization rules only for reusable workflows, skills, templates, or repeated procedures.
7. If the prompt is meant for an agent that can edit files or run tools, include concrete validation commands, sample checks, or acceptance criteria whenever possible.
8. If the original prompt contains conflict, missing critical information, or impossible constraints, either resolve with a conservative assumption or ask only the minimum necessary clarification.

## Output Contract

Return the optimized prompt first. Then add a short note only when useful:

- `主要改动`: mention the highest-impact changes.
- `使用建议`: mention how to fill placeholders, run validation, or choose optional blocks.
- `待确认`: list critical missing information only if it blocks reliable execution.

Do not over-explain obvious rewrites. Do not include a long critique unless the user asks for analysis.

## Default Templates

For most agent tasks, use this compact structure:

```text
# 任务

# 背景

# 要求

# 成功标准

# 验证方式

# 交付格式
```

For skill or reusable-capability prompts, use this compact structure:

```text
# 目标能力

# 触发条件

# 不触发条件

# 核心流程

# 资源组织

# 验证方式

# 交付格式
```

## Decision Rules

- Prefer concise task prompts over exhaustive checklists.
- Default to the lightest prompt strength that can execute the task reliably; do not ask the user to choose a strength unless it would materially change the output.
- Turn absolute prohibitions into decision rules unless the behavior is genuinely never allowed.
- Keep "run first, optimize second, generalize third" for one-off goals.
- Keep "trigger correctly, stay concise, generalize safely, validate with representative samples" for skills.
- Preserve domain-specific details that affect output quality; remove details that only reflect the source example's theme or wording.
- If the user asks for an abstract or reusable prompt structure, use placeholders and option buckets instead of a polished one-off rewrite.

## Reference

Read `references/prompt-optimization.md` only when the task needs prompt strength details, a rubric, a deeper diagnosis, a complex before/after rewrite, a reusable prompt template, or a metric-driven iteration loop.
