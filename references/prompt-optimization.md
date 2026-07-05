# Prompt Optimization Reference

Use this reference when a complex task prompt needs deeper structure, reusable templates, orchestration guidance, fallback wording, or quality review.

## Rubric

Score the optimized prompt against these dimensions:

1. Objective clarity: the agent can restate the desired outcome in one sentence.
2. Context sufficiency: required files, examples, background, constraints, and source materials are present or explicitly requested.
3. Actionability: the prompt tells the agent what to do, what to produce, and how to proceed when information is missing.
4. Verification: success can be checked by tests, commands, examples, screenshots, source review, rubric scoring, or observable acceptance criteria.
5. Complex but not bloated: the prompt includes the structure required for reliable execution without irrelevant process, repeated rules, or decorative language.
6. Coordination quality: main-agent and subagent responsibilities are separable, independently useful, and reviewable.
7. Capability honesty: the prompt does not claim subagent, browsing, file editing, or tool abilities unless the target executor supports them.
8. Conflict control: requirements do not contradict each other, and tradeoffs are resolved into clear priorities.
9. Output usability: the final response format is explicit enough to use directly.

## Common Fixes

- Replace vague goals with concrete deliverables and acceptance criteria.
- Replace "make it better" with named quality dimensions and observable checks.
- Replace long undifferentiated checklists with a supervised loop: research, plan, execute, validate, summarize.
- Replace "must be perfect" with testable pass/fail standards.
- Replace open-ended research with source quality, minimum evidence, and how findings affect execution.
- Replace generic "use subagents" with exact subtask boundaries, inputs, outputs, and review criteria.
- Replace unsupported multi-agent claims with a single-agent staged fallback.
- Replace endless iteration with stop conditions: threshold met, retry limit reached, no further gain, or clear blocker.
- Replace scenario-specific hacks with reusable rules when the user is building a workflow, skill, template, or repeated procedure.

## Length Targets

Treat length as a soft constraint. Execution clarity matters more than exact character count.

- Ordinary complex task prompt: 800-1800 Chinese characters.
- High-quality engineering, research, skill, template, or review prompt: 1500-3500 Chinese characters.
- Above 3500 Chinese characters: split into a main prompt plus appendix, acceptance table, examples, or reference material so key instructions are not buried.

Do not shorten by deleting goals, constraints, verification, or failure handling. Shorten by removing repetition, decorative language, redundant process narration, and requirements that belong in reference material.

## Complex Task Prompt Template

```text
# 任务
{one-sentence outcome}

# 背景
{only context required to execute the task reliably}

# 目标与关键结果
- {observable result}
- {observable result}
- {observable result}

# 输入与约束
- 输入：{files, links, drafts, data, examples, or missing inputs}
- 硬约束：{must-follow constraints}
- 优先级：{tradeoffs such as quality over speed, compatibility over novelty}

# 执行流程
1. 调研：{what to inspect, sources to use, facts to verify}
2. 计划：{how to decompose and define acceptance}
3. 执行：{what to change, create, analyze, or produce}
4. 验收：{tests, checks, review rubric, samples}
5. 总结：{final report content}

# 成功标准
- {pass/fail criterion}
- {pass/fail criterion}
- {pass/fail criterion}

# 验证方式
{commands, manual checks, review method, or reason verification cannot be automated}

# 失败处理与停止条件
- 最多重试 {N} 次同一失败点。
- 达到验收标准即停止。
- 信息不足、工具不可用或连续无收益时，输出阻塞原因、已验证事实、下一步所需信息。

# 交付格式
- 完成内容
- 关键决策
- 验证结果
- 剩余风险或待确认项
```

## Main-Subagent Supervision Template

Use this when the target executor supports subagents or equivalent delegated workers.

```text
# 执行方式
主智能体只负责目标保持、上下文整合、任务拆解、调度、验收、风险控制和最终汇总。不要把具体实现、长篇调研或重复验证堆在主智能体中。

# 子智能体职责
- 调研子智能体：读取指定资料，输出事实、证据、冲突点和建议，不改文件。
- 执行子智能体：在明确文件或模块边界内完成实现，输出变更摘要和验证结果。
- 审查子智能体：按验收标准检查遗漏、冲突、过度设计和风险，不重做实现。
- 验证子智能体：运行指定测试、样本或清单，报告通过项、失败项和复现信息。

# 协作规则
1. 主智能体先定义目标、成功标准、任务边界和共享上下文。
2. 只把可并行、可独立判断完成的任务交给子智能体。
3. 每个子智能体必须返回：输入、执行动作、发现、产物、验证结果、阻塞项。
4. 主智能体审查所有子结果，解决冲突，决定局部重试或进入下一阶段。
5. 不把内部调度日志写入最终交付，除非用户要求过程记录。
```

## Single-Agent Fallback Template

Use this when the target executor does not support subagents, or when the prompt will be pasted into a normal chat model.

```text
# 单智能体 fallback
当前环境不支持 subagent 时，不要声称派发子智能体。改为按阶段独立执行：

1. 调研阶段：只收集事实、约束和未知项，输出简短调研结论。
2. 计划阶段：把任务拆成可验收步骤，列出成功标准和验证方式。
3. 执行阶段：按计划逐项完成，遇到冲突先回到目标和约束判断。
4. 验收阶段：按成功标准逐项检查，失败则定位原因并局部修复。
5. 总结阶段：只交付完成内容、验证结果、剩余风险和待确认项。

每一阶段结束后自检：本阶段产物是否支持最终目标，是否引入无关范围，是否有未处理阻塞。
```

## Skill Or Reusable Capability Template

```text
# 目标能力
{what reusable capability the skill gives the agent}

# 触发条件
- {phrases, task types, artifacts, or contexts that should trigger it}

# 不触发条件
- {nearby requests that should route elsewhere or be handled directly}

# 核心工作流
1. {first required action}
2. {second required action}
3. {validation or delivery action}

# 主-子智能体执行策略
- 主智能体：{coordination and acceptance role}
- 子智能体：{only if supported and useful}
- fallback：{single-agent staged behavior}

# 资源组织
- SKILL.md: {core rules and navigation only}
- references/: {long rules, examples, rubrics}
- scripts/: {repeatable deterministic automation}
- assets/: {templates or reusable files}

# 验证方式
- 正向样例：{should trigger and expected behavior}
- 边界样例：{should trigger only under specific condition}
- 反向样例：{should not trigger}

# 交付格式
- skill 文件结构
- 核心内容
- 验证结果
- 已知限制
```

## Quality Checklist

Before delivering the optimized prompt, check:

- The goal can be restated in one sentence.
- Acceptance criteria are observable.
- Required context and inputs are named.
- Main-agent responsibilities are supervisory, not mixed with all execution details.
- Subagent tasks are independent, bounded, and reviewable.
- Research requirements specify source quality or inspection targets.
- Verification is concrete or the lack of automation is explained.
- Stop conditions and retry limits are present for iterative work.
- The prompt avoids fake subagent/tool capabilities.
- The prompt contains no short/closed-loop/complex strength labels.
- The prompt has no duplicate rules, conflicting constraints, or generic polish language.
- Any appendix or examples are separated from the main prompt when the prompt is long.

## Failure Handling Pattern

```text
# 失败处理
- 若验证失败，先定位失败属于目标理解、上下文缺失、计划拆解、实现、工具、数据、输出格式还是验收标准问题。
- 只做局部修复并重跑相关验证，不重写无关部分。
- 同一失败点最多重试 {N} 次。
- 达到重试上限、缺少关键输入、工具不可用或连续无收益时停止，并输出：阻塞原因、已验证事实、尝试过的修复、下一步需要的信息或权限。
```

## Stop Conditions

Stop iterating when one of these is true:

- The optimized prompt preserves the user's intent and satisfies the rubric.
- The prompt has enough structure for reliable execution and more detail would mainly add noise.
- Remaining uncertainty requires user input.
- The task should become a skill, reference file, or implementation plan instead of a single prompt.
- A validation loop reaches its threshold, retry limit, or a clearly reported blocker.
