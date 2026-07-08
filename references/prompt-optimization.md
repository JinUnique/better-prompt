# Prompt Optimization Reference

Use this reference when a Codex `/goal` prompt needs source anchors, deeper structure, reusable field patterns, orchestration guidance, failure-handling patterns, or quality review.

## Source Anchors

- OpenAI Codex manual and "Using Goals in Codex": goal text acts as starting prompt plus completion criteria; a goal defines what should be true, how success is checked, and what constraints stay intact.
- Anthropic prompt engineering and Claude Code best practices: write explicit instructions, separate context from instructions, specify output format, and give the agent a check it can run.
- Hermes Agent SOUL.md guide: SOUL.md defines durable identity; project details belong in AGENTS.md; temporary task contracts belong in the current request or referenced files.
- Public high-confidence social discussions on X/Reddit/Hacker News: use only as pressure tests for whether agent users repeatedly punish vague, unverifiable, bloated prompts.

## Source-grounded Behavior Model

These are the concrete rules to apply. Do not cite source labels without translating them into a decision rule.

### OpenAI Codex rules

- Goal gives Codex a completion condition: the prompt must define what becomes true, how success is checked, and what must not regress.
- A strong goal is a compact contract, not a bigger prompt. Required fields: outcome, verification surface, constraints, boundaries, iteration policy, blocked stop condition.
- Codex produces better work when it can verify: include tests, commands, reproduction steps, artifacts, screenshots, or source-review evidence.
- Complex work should be split into focused, reviewable steps. If success criteria are unclear, ask for or draft a `/plan` before execution.
- Durable repository guidance belongs in AGENTS.md; `/goal` should reference durable files instead of copying stable rules into the task text.

### Anthropic rules

- Be explicit about behavior, context, constraints, and output format; do not rely on "be better", "be professional", or "above and beyond".
- Give the agent a check it can run: tests, build, screenshot comparison, sample diff, data validation, source citation check, or acceptance checklist.
- Separate instructions, context, examples, and variable inputs with headings or tags when the prompt contains multiple content types.
- Prefer positive target behavior over negative-only constraints: state what the answer must contain, in what order, at what granularity.
- Examples are useful only when they remove ambiguity; otherwise they add style drift and context cost.

### Hermes Agent rules

- SOUL.md is identity; AGENTS.md is project context; /goal is transient completion contract.
- Keep durable agent governance stable, broad, and short; put project workflow in AGENTS.md; put one-off execution boundaries in `/goal`.
- If a requested prompt starts mixing persona, repo commands, file paths, and task acceptance, split by surface instead of writing a single overgrown instruction.
- Long or project-specific background should become a referenced file, plan, issue, or acceptance table, not the main goal body.

### Public social-review rules

- Treat X, Reddit, and Hacker News as adversarial review signals, not authority. A rule only enters the skill if it survives official-source mapping and repeated user pain.
- Repeated high-signal preferences: short hard constraints, explicit priority order, visible evidence, no self-attestation, no vague "try your best", and clear stop conditions.
- Repeated failure modes: prompts that ask for "research thoroughly" without source quality, "build fully" without tests, "make it better" without acceptance criteria, or "use agents" without reviewable subtask boundaries.

## Goal Prompt Style

For `/goal`, prefer:

- one durable objective over many loosely related desires
- acceptance evidence over self-attestation
- scope boundaries over implementation micromanagement
- preference ordering over value statements
- hard stop conditions over "try your best"
- source links or file references over copied background
- concise operational clauses over explanatory paragraphs

## Rubric

Score the optimized prompt against these dimensions:

1. Objective clarity: the agent can restate the desired outcome in one sentence.
2. Context sufficiency: required files, examples, background, constraints, and source materials are present or explicitly requested.
3. Actionability: the prompt tells the agent what to do, what to produce, and how to proceed when information is missing.
4. Verification: success can be checked by tests, commands, examples, screenshots, source review, rubric scoring, or observable acceptance criteria.
5. Evidence-based completion: for autonomous loops, the prompt names what evidence proves done and what evidence means blocked.
6. Context economy: the prompt keeps high-signal instructions in the main body and moves bulky background, examples, or long checklists to referenced files or appendices.
7. Complex but not bloated: the prompt includes the structure required for reliable execution without irrelevant process, repeated rules, or decorative language.
8. Coordination quality: main-agent and subagent responsibilities are separable, independently useful, and reviewable.
9. Capability honesty: the prompt does not claim browsing, file editing, or tool abilities unless the target executor supports them.
10. Conflict control: requirements do not contradict each other, and tradeoffs are resolved into clear priorities.
11. Output usability: the final response format is explicit enough to use directly.

## Common Fixes

- Replace vague goals with concrete deliverables and acceptance criteria.
- Replace "make it better" with named quality dimensions and observable checks.
- Replace long undifferentiated checklists with a supervised loop: research, plan, execute, validate, summarize.
- Replace "must be perfect" with testable pass/fail standards.
- Replace open-ended research with source quality, minimum evidence, and how findings affect execution.
- Replace generic "use subagents" with exact subtask boundaries, inputs, outputs, and review criteria.
- Replace endless iteration with stop conditions: threshold met, retry limit reached, no further gain, or clear blocker.
- Replace "bigger prompt" instincts with a compact completion contract plus references to source files, specs, issues, or artifacts.
- Replace agent self-attestation with evidence mapping: criterion -> check/output -> status.
- Replace scenario-specific hacks with reusable rules only when the user is building a repeated workflow; otherwise keep the goal task-specific.

## Length Targets

Treat length as an operating constraint. The main `/goal` must stay compact enough to keep the completion contract visible.

- Codex `/goal` simple target: 1-3 sentences / 50-150 Chinese characters.
- Codex `/goal` ordinary engineering target: 8-20 lines / 300-800 Chinese characters.
- Codex `/goal` complex long-running target: still 300-800 Chinese characters in the main goal.
- 超过 800 中文字必须拆：move bulky context, examples, research notes, acceptance tables, or implementation plans into `PLAN.md`, `SPEC.md`, issue, reference file, or appendix, then point the `/goal` at them.
- If the user asks for more detail, keep the main `/goal` within 800 Chinese characters and place extra detail in referenced material or a clearly separate appendix.

Do not shorten by deleting goals, constraints, verification, or failure handling. Shorten by removing repetition, decorative language, redundant process narration, and requirements that belong in reference material.

## Goal Prompt Pattern

Use `/goal` when the task has a durable objective, an evidence-based finish line, and an uncertain multi-step path. Do not use it for one-line edits, simple explanations, taste-heavy choices, or vague "make it better" work.

The minimum contract is:

```text
/goal <one durable objective>. Done means <observable outcome>, verified by <tests/commands/artifact/evidence>, while preserving <constraints>. Use only <allowed scope/resources>. Between iterations, <record evidence and choose the next best experiment>. If blocked, verification cannot run, scope expansion is required, or no valid path remains, stop and report attempted paths, evidence, blocker, and exact input needed.
```

For complex work, prefer short labeled lines:

```text
/goal <single objective>
Context: <essential context or file/spec links only>
Scope: <allowed files/tools/resources>; ask before <risky actions>; never <forbidden actions>
Verification: <primary command/check/artifact>; evidence required <logs/report/diff/screenshot>
Iteration: <smallest safe slice, rerun narrow check, record changed evidence, pick next different strategy>
Stop if: <blocked data/tool/permission, no-progress, budget/time cap, human decision required>
Done when:
1. <observable criterion + evidence>
2. <observable criterion + evidence>
Final report: DONE/PARTIAL/BLOCKED plus changed files, checks run, evidence map, risks, next step
```

Strong `/goal` prompts answer four questions:

1. What single outcome should become true?
2. What evidence proves it is true?
3. What boundaries and constraints must stay intact while the agent works?
4. When should the agent stop instead of continuing to spend tokens or take risk?

Weak `/goal` prompts usually fail because they are vague, broad, or self-judging:

- Bad: `/goal improve auth`
- Better: `/goal Raise src/auth coverage from 38% to 75%, only edit src/auth and tests, stop when npm test passes and the coverage threshold is met.`

If the source context is large, write or reference a plan first:

```text
/goal Execute Phase 1 from docs/migration-plan.md for the auth module. Done when npm test and auth e2e pass, public APIs are unchanged, and the final report maps each Phase 1 acceptance item to command output or diff evidence. Stop if the plan conflicts with current code or requires dependency/production changes.
```

## Main-Subagent Supervision Clause

Use this only when subagents materially improve the goal:

```text
Execution: Main agent owns objective, scope control, task decomposition, subagent handoff, review, risk decisions, and final acceptance. Use subagents only for independent research, implementation, review, or verification tasks with clear input, output, and acceptance criteria. Main agent must verify subagent claims against evidence before reporting DONE.
```

Subagent clauses must name:

- input: files, links, data, or question
- output: facts, patch, report, test result, or finding list
- boundary: files/tools/resources allowed
- acceptance: how the main agent checks it
- stop condition: when the subagent should return blocked instead of guessing

## Failure Handling Pattern

```text
Failure handling: If verification fails, classify the failure as goal ambiguity, missing context, decomposition error, implementation error, tool/data failure, output-format issue, or acceptance mismatch. Make one local fix, rerun the narrowest relevant check, and record changed evidence. Stop after <N> failed attempts at the same point or when key input/tool/permission is missing; report blocker, verified facts, attempted fixes, and exact unlock needed.
```

## Quality Checklist

Before delivering the optimized prompt, check:

- The goal can be restated in one sentence.
- Acceptance criteria are observable.
- Required context and inputs are named.
- Research requirements specify source quality or inspection targets.
- Verification is concrete or the lack of automation is explained.
- Stop conditions and retry limits are present for iterative work.
- Autonomous-loop prompts define evidence-based completion and do not rely on the agent declaring success without checks.
- `/goal` prompts keep bulky background out of the main goal text and point to files/specs when needed.
- Subagent tasks, if any, are independent, bounded, and reviewable.
- The prompt has no duplicate rules, conflicting constraints, abstract source labels, generic polish language, or migrated unrelated persistent-instruction requirements.
