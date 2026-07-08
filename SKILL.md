---
name: better-prompt
description: 'Use when the user asks to write, optimize, shorten, or critique an existing Codex /goal task prompt, or turn requirements into a high-delivery Codex /goal task prompt, including Chinese requests such as 优化 /goal 任务提示词, 改写成 /goal, 设计 Codex /goal, agent 可执行 /goal. Do not use for ordinary prose editing, simple translation, casual questions, creative media prompts, or writing global/persona/project instruction files.'
---

# Better Prompt

## Core Rule

唯一目标：把复杂或高要求需求改写成高交付的 Codex `/goal` 任务提示词。目标不是让文字更好看，而是让下游 agent 在长跑任务中稳定识别目标、边界、验证、停止条件和交付格式。

默认输出可直接粘贴的 `/goal` 正文。除非用户明确要求评审或解释，否则不要退回泛化任务提示词、普通文案、全局指令、SOUL、AGENTS.md 或 persona 文件。

## Source-Derived Rules

- OpenAI/Codex: /goal 同时写起始目标和完成条件；强 goal 必含 Outcome / Verification surface / Constraints / Boundaries / Iteration policy / Blocked stop condition。
- OpenAI/Codex: 复杂工作拆成可验证小步；成功标准不清时建议先 `/plan`；完成声称必须绑定命令、产物或证据。
- Anthropic: 明确写行为、上下文、输出格式和约束；给 agent 可运行验证；用正向目标行为替代反向禁令。
- Anthropic: 长上下文用分区或标签隔开；资料、指令、示例、变量输入不可混成一团。
- Hermes Agent: SOUL 写身份，AGENTS 写项目规则，/goal 写本次完成合约；持久人格、项目规范、临时目标必须分层。
- 社媒信号只当压力测试：只吸收 X/Reddit/Hacker News 反复出现且能映射到官方规则的硬约束、偏好排序、证据映射、停止条件。

## Workflow

1. 判断请求是否需要 `/goal`：多步、长跑、高风险、需要验证或需要 agent 自主推进时才使用；轻量改写直接建议不用。
2. 若用户要写全局指令、SOUL、AGENTS.md 或 persona，本技能只可写“让 Codex 修改这些文件的 `/goal`”，不可直接产出全局指令正文。
3. 提炼一个 durable objective：只保留真正要变成事实的结果，不把背景、愿景、资料堆进目标。
4. 写出完成合约：outcome、verification、constraints、boundaries、iteration、stop if、done when、final report。
5. 写出偏好排序：先做什么、拒绝什么、默认路线、何时改变路线；用可执行决策替代价值宣言。
6. 写出风险护栏：资金损失、隐私泄露、不可逆操作、生产变更、范围扩张必须停下并请求确认。
7. 若任务适合多智能体，只描述主智能体监督验收与子智能体输入/输出/边界；不要展开调度日志。
8. 审查每一行：必须改变 agent 决策、验收或停止条件；删掉只表达风格、愿望、解释或来源背书的句子。

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

## Output Contract

先交付优化后的 `/goal` 提示词。然后仅在有用时追加极短说明：

- `主要改动`: 只列 1-3 个最高影响的结构性改动。
- `使用建议`: 只说明占位符、验证命令或是否需要先 `/plan`。
- `待确认`: 只列阻碍可靠执行的关键信息。

不要输出长篇点评。不要把内部评审、评分表、来源综述或推理过程混进可复制的 `/goal` 提示词。

## Decision Rules

- `/goal` 是唯一交付对象；所有模板和解释都服务于高交付 goal，不服务于“更完整的文章”。
- 触发边界必须收窄；如果任务本身很轻，应直接建议不使用本技能。
- 默认先写可验证完成条件，再补上下文；不要先堆资料再找目标。
- 定性要求必须落到可执行决策或可观察验收上。
- 子任务必须独立、有清晰输入输出、能被主智能体验收；不能为了显得复杂而拆分。
- 失败处理必须包含重试上限、阻塞条件和报告格式。
- 字数默认 300-800 中文字；简单 goal 50-150 字；超过 800 中文字必须拆成 `PLAN.md`、`SPEC.md`、issue、验收表或参考材料，再让 `/goal` 引用它们。
- 不把历史对话中的文风、清单格式或全局规则需求迁移进当前 `/goal`；只有当前任务明确要求时才保留相应格式。
- 如果用户给出具体需求，交付可直接复制执行的完整 `/goal`；如果信息不足，输出最小待确认项而不是填充假设。

## Reference

Read `references/prompt-optimization.md` when the task needs source anchors, reusable field patterns, deeper rubrics, main-subagent orchestration details, failure-handling patterns, or a final quality checklist.
