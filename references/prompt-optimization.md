# Prompt Optimization Reference

Use this reference when a prompt rewrite needs deeper structure, scoring, or reusable templates.

## Rubric

Score the prompt against these dimensions:

1. Objective clarity: the agent can state the desired outcome in one sentence.
2. Context sufficiency: required files, examples, background, and constraints are present.
3. Actionability: the prompt tells the agent what to do next, not just what to think about.
4. Verification: success can be checked by tests, examples, commands, screenshots, review criteria, or observable acceptance standards.
5. Scope control: the prompt avoids unnecessary features, research, abstraction, or process.
6. Conflict control: requirements do not contradict each other.
7. Output usability: final answer format is explicit enough for direct use.

## Common Fixes

- Replace vague goals with concrete deliverables.
- Replace "make it better" with named quality dimensions.
- Replace long process checklists with a compact workflow plus success criteria.
- Replace "must be perfect" with testable acceptance criteria.
- Replace "do research" with source count, source quality, and how findings affect implementation.
- Replace "independent review" with who reviews, what they see, and which rubric they use.
- Replace "generic skill" with trigger conditions, non-trigger conditions, workflow, resources, and validation.
- Replace open-ended iteration with a loop: evaluate, diagnose, fix, rerun, then stop by threshold, attempt limit, or clear blocker.
- Replace scenario-specific fixes with reusable rules unless the user explicitly asks for a one-off answer.

## Prompt Strength

Choose the smallest sufficient prompt strength before rewriting. Infer the level from the user's draft by default. Do not ask the user to choose unless the draft is ambiguous and the choice would materially change the result.

Treat length ranges as soft targets. Execution clarity matters more than exact character count.

### Short Task

Use for ordinary one-off prompts, simple rewrites, and single-step agent tasks.

Default length: about 200-500 Chinese characters.

Focus on:
- Task.
- Context.
- Requirements.
- Success criteria.
- Output format.

Do not add independent review, self-iteration, long research, or complex metric tables by default.

For short tasks, success criteria can replace a separate verification section unless the task itself needs explicit checks.

### Closed-Loop Goal

Use for tasks that require execution, verification, fixing, rerunning, or measurable completion.

Default length: about 500-1500 Chinese characters.

Focus on:
- Success criteria.
- Verification method.
- Failure handling.
- Rerun rules.
- Stop conditions.

May add lightweight review, process notes, metrics, or thresholds when useful.

### Complex Requirement

Use for skill creation, complex engineering work, reusable capability building, multi-stage research, or batch evaluation.

Default length: about 1500-4000+ Chinese characters when needed.

Focus on:
- Research requirements.
- Resource organization.
- Independent review.
- Self-iteration.
- Generalization constraints.
- Acceptance criteria.

May add subagent review, rubrics, sample validation, and multi-round iteration records.

For complex prompts, include a short instruction for how the downstream agent should consume the prompt: read the full task once, execute by modules, revisit relevant requirements during implementation, and verify against success criteria before delivery.

## Compact Agent Prompt Template

```text
# 任务
{one-sentence outcome}

# 背景
{only context required to complete the task}

# 要求
- {hard requirement}
- {constraint}
- {preference or priority}

# 成功标准
- {observable criterion}
- {observable criterion}
- {observable criterion}

# 验证方式
{commands, sample checks, review criteria, or "无法自动验证时说明原因"}

# 交付格式
- 完成了什么
- 修改了什么
- 验证结果
- 剩余风险
```

## Skill Prompt Template

```text
# 目标能力
{what reusable capability the skill gives the agent}

# 触发条件
- {user phrases, task types, materials, or contexts that should trigger it}

# 不触发条件
- {nearby tasks that should not trigger it}

# 核心流程
1. {first action}
2. {second action}
3. {validation or delivery action}

# 资源组织
- SKILL.md: {core rules and navigation only}
- references/: {long rules, examples, rubrics}
- scripts/: {repeatable deterministic automation}
- assets/: {templates or reusable files}

# 验证方式
- {typical sample}
- {edge sample}
- {non-trigger sample}

# 交付格式
- skill 文件结构
- 核心内容
- 验证结果
- 已知限制
```

## When To Add Optional Blocks

Add research only when external facts, market examples, platform rules, or best practices materially affect the result.

Add independent review only when the task is high-stakes, complex, or likely to suffer from self-evaluation bias.

Add generalization only when the user is building a reusable workflow, skill, template, or repeated operating procedure.

Add examples only when they disambiguate style, format, or failure modes.

Add metric-driven iteration only when the task has generated artifacts, tests, baselines, samples, or other observable quality signals. Keep it out of ordinary rewrite requests.

## Metric-Driven Iteration Block

Use this optional block for evaluation-heavy prompts, batch generation, coding agents with tests, prompt optimization experiments, and workflows that must improve artifacts until measurable criteria are met.

```text
# 评测指标
| 指标 | 阈值 | 检查方式 | 失败时必须输出 |
|---|---|---|---|
| {metric} | {threshold} | {test, comparison, rubric, or command} | {missing items, failed cases, or error list} |

# 迭代闭环
1. 执行当前需求或样本。
2. 按评测指标判断是否通过。
3. 未通过时，逐项归因失败样本，定位到输入理解、资料召回、prompt、代码、工具、数据或输出格式。
4. 只修复可泛化问题，避免针对具体题面、字段、文案或单个样本硬编码。
5. 修复后立即重跑同一需求或样本，记录效果变化。
6. 达标后进入下一个需求；达到重试上限仍未达标时，记录失败原因和剩余风险。

# 重试限制
- 单个需求或样本最多尝试 {N} 次。
- 达标即停；连续无收益或遇到明确阻塞时停止并报告原因。

# 过程记录
- 问题：
- 归因：
- 修改：
- 重跑结果：
- 收益：
```

Do not copy domain-specific metrics from the source prompt unless they match the user's actual task. Convert them into task-specific slots.

## Prompt Review Checklist

After rewriting a prompt that will itself drive implementation or iteration, review it once for:

- Duplicate rules.
- Conflicting requirements.
- Bloated process steps.
- Missing success criteria.
- Missing verification or stop condition.
- Hard-coded details that should be generalized.
- Generic "be professional" language that does not change execution.
- Requirements that belong in a reference file instead of the task prompt.

## Stop Conditions

Stop iterating when one of these is true:

- The rewritten prompt satisfies the rubric and preserves the user's intent.
- Remaining uncertainty requires user input.
- Additional detail would mainly increase prompt length without improving execution.
- The prompt is now better handled as a skill/reference file instead of a single task prompt.
- A metric-driven loop reaches its threshold, attempt limit, or a clearly reported blocker.
