---
name: better-prompt
description: 'Optimize complex or high-quality task prompts for agent execution. Use when the user asks to optimize a complex task prompt, rewrite a requirement into an executable agent prompt, create a high-quality Codex /goal or workflow-agent prompt, design prompts for research implementation review validation loops, or turn a skill/template/reviewer workflow into clear goals, constraints, success criteria, verification, and delivery format. Also use for Chinese requests such as 优化任务提示词, 改写成高质量执行提示词, 把这个需求整理成 agent 可执行提示词, 设计多智能体执行提示词, or 设计 /goal 提示词. Do not use for ordinary prose editing, one-sentence rewrites, simple translation, casual questions, lightweight prompt polish, or creative media prompts that should route to a domain-specific image video or music prompt skill.'
---

# Better Prompt

## Core Rule

只优化复杂或高质量交付导向的任务提示词。目标不是让文字更好看，而是让下游 agent 更稳定地理解目标、拆解工作、执行验证并交付可验收结果。

默认把任务提示词改写为主-子智能体监督式执行闭环：主智能体负责目标保持、需求澄清、任务拆解、子任务调度、验收标准、风险控制和最终汇总；子智能体负责独立调研、实现、审查或验证。

当目标执行器是 Codex `/goal` 或类似长跑 agent loop 时，提示词应写成紧凑的完成合约，而不是更大的 prompt：明确 outcome、verification surface、constraints、boundaries、iteration policy、blocked stop condition。把长背景、资料、样例和清单移到文件、issue、spec 或 reference 中，由 agent 按需读取；`/goal` 正文只保留足以执行和验收的高信号信息。

复杂不等于冗长。删除重复规则、空泛形容词、无法验收的要求、无关过程叙述和 generic "be professional" 语言；保留会影响执行质量的目标、背景、约束、输入、验收、验证和失败处理。

## Workflow

1. 判断是否应使用本技能。仅当用户要优化复杂任务、高质量交付、agent 执行、工作流、reviewer、skill 或模板提示词时继续；普通文案润色、一句话改写、翻译、闲聊问题和轻量 prompt polish 不应承接。
2. 识别目标执行器：Codex `/goal`、Codex 普通 prompt、coding agent、general workflow agent、reviewer、research agent 或 skill authoring agent。
3. 提炼真实目标、关键结果、上下文、输入材料、硬约束、偏好、交付物和验收信号。若原始需求隐藏错误前提或目标不清，先纠正或列出最小待确认项。
4. 将需求拆成可执行闭环：调研 -> 计划 -> 执行 -> 验收 -> 总结。为每一阶段写清楚产物、通过标准和失败处理。若是 `/goal`，不要写成长篇阶段说明；压缩为“目标 + 证据面 + 边界 + 迭代策略 + 阻塞停止”的合约。
5. 设计主-子智能体协作：主智能体只承担目标保持、监督管理、审查验收和整合；子智能体只承担边界清晰、可独立验证的调研、实现、审查或测试任务。
6. 加入验证方式、停止条件和阻塞报告。能运行工具时给出命令、样本检查或验收表；无法自动验证时要求说明原因、已验证事实和剩余风险。
7. 审查输出提示词，确保没有分级残留、重复规则、冲突要求、无效流程、硬编码样例或与任务无关的附录。

## Codex `/goal` Contract

Use this compact structure when the output will be pasted after `/goal`:

```text
/goal <one durable objective>. Done means <observable outcome>, verified by <tests/commands/artifact/evidence>, while preserving <constraints>. Use only <allowed scope/resources>. Between iterations, <record evidence and choose the next best experiment>. If blocked, verification cannot run, scope expansion is required, or no valid path remains, stop and report attempted paths, evidence, blocker, and exact input needed.
```

For complex `/goal` work, keep the same contract but expand into short labeled lines instead of a long paragraph:

```text
/goal <single objective>
Context: <only essential context or file/spec links>
Scope: <allowed files/tools/resources>; ask before <risky actions>; never <forbidden actions>
Verification: <primary command/check/artifact>; evidence required <logs/report/diff/screenshot>
Iteration: <smallest safe slice, rerun narrow check, record changed evidence, pick next different strategy>
Stop if: <blocked data/tool/permission, no-progress, budget/time cap, human decision required>
Done when: <numbered observable acceptance criteria>
Final report: DONE/PARTIAL/BLOCKED plus changed files, checks run, evidence map, risks, next step
```

## Default Structure

```text
# 任务

# 背景

# 目标与关键结果

# 执行方式
## 主智能体职责
## 子智能体职责

# 流程
## 调研
## 计划
## 执行
## 验收
## 总结

# 成功标准

# 验证方式

# 失败处理与停止条件

# 交付格式
```

For skill or reusable-capability prompts, use this structure:

```text
# 目标能力

# 触发条件

# 不触发条件

# 核心工作流

# 主-子智能体执行策略

# 资源组织

# 验证方式

# 验收样例

# 交付格式
```

## Output Contract

先交付优化后的提示词。然后仅在有用时追加极短说明：

- `主要改动`: 只列最高影响的结构性改动。
- `使用建议`: 说明需要替换的占位符、验证命令或如何启用 subagent 协作。
- `待确认`: 只列阻碍可靠执行的关键信息。

不要输出长篇点评。不要把内部评审、评分表或推理过程混进可复制的优化提示词。

## Decision Rules

- 默认面向复杂任务和高质量交付，不为简单润色降低强度。
- 触发边界必须收窄；如果任务本身很轻，应直接建议不使用本技能。
- 默认使用主-子智能体执行结构，由主智能体保持目标并监督验收。
- 子任务必须独立、有清晰输入输出、能被主智能体验收；不能为了显得复杂而拆分。
- 定性要求必须落到可执行决策或可观察验收上。
- 失败处理必须包含重试上限、阻塞条件和报告格式。
- 字数是软约束。`/goal` 提示词默认短而完整：简单目标 1-3 句 / 50-150 中文字；中等工程任务 8-20 行 / 300-800 中文字；复杂长跑任务最多约 1 屏 / 800-1500 中文字。超过 1500 中文字时优先拆为 `PLAN.md`、`SPEC.md`、issue、验收表或参考材料，再让 `/goal` 引用它们。
- 非 `/goal` 的普通复杂任务目标 800-1800 个中文字；高质量工程、研究、skill 或模板任务目标 1500-3500 个中文字；超过 3500 字时拆成主提示词加附录、验收表或参考资料。
- 如果用户要求抽象结构，使用占位符和选项槽；如果用户给出具体需求，交付可直接复制执行的完整提示词。

## Reference

Read `references/prompt-optimization.md` when the task needs reusable templates, a deeper rubric, main-subagent orchestration details, failure-handling patterns, or a final quality checklist.
