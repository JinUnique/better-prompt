---
name: better-prompt
description: 'Use when the user asks to write, optimize, shorten, critique, or turn requirements into a high-delivery Codex /goal task prompt or agent goal, including Chinese requests such as 优化任务提示词, 改写成 /goal, 设计 /goal 提示词, agent 可执行目标. Do not use for ordinary prose editing, simple translation, casual questions, creative media prompts, or writing global/persona/project instruction files.'
---

# Better Prompt

## Core Rule

唯一目标：把复杂或高要求需求改写成高交付的 Codex `/goal` 任务提示词。目标不是让文字更好看，而是让下游 agent 在长跑任务中稳定识别目标、边界、验证、停止条件和交付格式。

默认输出可直接粘贴的 `/goal` 正文。除非用户明确要求普通 prompt、skill、模板或评审提示词，否则不要退回泛化任务提示词。

行为规范来自 OpenAI Codex、Anthropic、Hermes Agent 官方指导，以及 X、Reddit、Hacker News 等多平台高置信评价的共识。把这些来源只压缩为 `/goal` 决策规则：单一目标、可验收证据、范围边界、偏好排序、风险停止、阻塞报告。

复杂不等于冗长。删除重复规则、空泛形容词、过程叙事、内部评分和 generic "be professional" 语言；保留会改变 agent 行为的目标、上下文、约束、偏好排序、验证、风险护栏和阻塞报告。

## Workflow

1. 判断是否是 `/goal`、长跑 agent 目标或 agent 可执行目标；是则使用本技能，不是则建议不用。
2. 提炼一个 durable objective：只保留真正要变成事实的结果，不把背景、愿景、资料堆进目标。
3. 写出完成合约：outcome、context、scope、verification、iteration、stop if、done when、final report。
4. 写出偏好排序：优先什么、拒绝什么、默认怎么做、除非什么才改变路线。
5. 写出风险护栏：资金损失、隐私泄露、不可逆操作、生产变更、范围扩张必须停下并请求确认。
6. 若任务适合多智能体，只描述主智能体监督验收与子智能体边界；不要展开调度日志。
7. 审查每一行：必须改变 agent 决策、验收或停止条件；删掉只表达风格、愿望或解释的句子。

## Codex `/goal` Contract

Use this compact structure when the output will be pasted after `/goal`:

```text
/goal <one durable objective>. Done means <observable outcome>, verified by <tests/commands/artifact/evidence>, while preserving <constraints>. Use only <allowed scope/resources>. Between iterations, <record evidence and choose the next best experiment>. If blocked, verification cannot run, scope expansion is required, or no valid path remains, stop and report attempted paths, evidence, blocker, and exact input needed.
```

For complex `/goal` work, keep the same contract but expand into short labeled lines:

```text
/goal <single objective>
Context: <only essential context or file/spec links>
Scope: <allowed files/tools/resources>; ask before <risky actions>; refuse <forbidden actions>
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

先交付优化后的 `/goal` 提示词。然后仅在有用时追加极短说明：

- `主要改动`: 只列 1-3 个最高影响的结构性改动。
- `使用建议`: 只说明占位符、验证命令或是否需要先 `/plan`。
- `待确认`: 只列阻碍可靠执行的关键信息。

不要输出长篇点评。不要把内部评审、评分表、来源综述或推理过程混进可复制的 `/goal` 提示词。

## Decision Rules

- `/goal` 是最高优先级分支；所有模板和解释都服务于高交付 goal，不服务于“更完整的文章”。
- 触发边界必须收窄；如果任务本身很轻，应直接建议不使用本技能。
- 默认使用主-子智能体执行结构，由主智能体保持目标并监督验收；但只在目标里写必要边界。
- 子任务必须独立、有清晰输入输出、能被主智能体验收；不能为了显得复杂而拆分。
- 定性要求必须落到可执行决策或可观察验收上。
- 失败处理必须包含重试上限、阻塞条件和报告格式。
- 字数默认 300-800 中文字；简单 goal 50-150 字；复杂长跑最多约 1 屏。超过 800 字时优先拆成 `PLAN.md`、`SPEC.md`、issue、验收表或参考材料，再让 `/goal` 引用它们。
- 不把历史对话中的文风、清单格式或全局规则需求迁移进当前 `/goal`；只有当前任务明确要求时才保留相应格式。
- 如果用户给出具体需求，交付可直接复制执行的完整 `/goal`；如果信息不足，输出最小待确认项而不是填充假设。

## Reference

Read `references/prompt-optimization.md` when the task needs reusable templates, a deeper rubric, main-subagent orchestration details, failure-handling patterns, or a final quality checklist.
