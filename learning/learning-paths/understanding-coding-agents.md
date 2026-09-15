# Understanding Coding Agents

> AI Use Disclosure: Codex was used to research, organize, and draft this guide.

How coding agents use context, tools, and feedback, and how to assess their work.

**Background:** Familiarity with reading code, repositories, tests, and reviewing changes. Optional: [Neural networks and language models](neural-networks-and-language-models.md).

## 1. Understand the model

- [How language models work](../../foundations/language-models/how-language-models-work.md) — generation, assistant training, and why convincing output can be wrong.
- [Reasoning models](../../foundations/language-models/reasoning-models.md) — intermediate work and its relationship to tool execution and evidence.

## 2. Picture the whole system

- [How agents work](../../foundations/agent-loops-and-autonomy/how-agents-work.md) — the model, runtime, tools, environment, and action loop.

## 3. Understand what the agent can see

- [What context engineering means](../../foundations/context-and-memory/context-engineering-and-memory.md#what-context-engineering-means) — selecting the information available to the model at each step.
- [State, memory, compaction, and caching](../../foundations/context-and-memory/context-engineering-and-memory.md#state-memory-compaction-and-caching) — preserving decisions and working state across a task.

## 4. Connect procedures to actions

- [Skills, tools, hooks, and delegation](../../foundations/tool-use-and-integrations/skills-tools-hooks-and-delegation.md) — procedures, operations, events, and delegated subtasks.

## 5. Follow one task from request to result

- [A worked example: fixing a browser interface](../../use-cases/software-development/agentic-coding-best-practices-2026-09.md#a-worked-example-fixing-a-browser-interface) — skill selection, investigation, reproduction, editing, and verification.

## 6. Understand feedback and completion

- [Harness engineering and executable feedback](../../use-cases/software-development/agentic-coding-best-practices-2026-09.md#harness-engineering-and-executable-feedback) — how the environment gives an agent useful evidence about its work.
- [Current working practices](../../use-cases/software-development/agentic-coding-best-practices-2026-09.md#current-working-practices) — task clarity, context selection, incremental changes, and review.
- [What the evidence does and does not establish](../../use-cases/software-development/agentic-coding-best-practices-2026-09.md#what-the-evidence-does-and-does-not-establish) — how to interpret claims about workflow effectiveness and productivity.

## 7. Bring human understanding into the picture

1. [Geoffrey Litt: understanding as participation](../../use-cases/software-development/practitioner-workflows-and-commentary-2026-09.md#understanding-as-participation-geoffrey-litt) — why understanding a change matters for deciding what comes next.
2. [Matt Pocock: a complete development workflow](../../use-cases/software-development/practitioner-workflows-and-commentary-2026-09.md#a-complete-development-workflow-matt-pocock) — how clarification, implementation, and review connect in practice.
3. [Dex Horthy: keeping teams aligned in complex codebases](../../use-cases/software-development/practitioner-workflows-and-commentary-2026-09.md#keeping-teams-aligned-in-complex-codebases-dex-horthy) — how research, plans, and context management support shared understanding.

[More videos](../../use-cases/software-development/practitioner-workflows-and-commentary-2026-09.md#videos-improving-quality-and-human-understanding) — software fundamentals, testing, and continuous feedback.

## Explore different working styles

- [Published practitioner workflows](../../use-cases/software-development/practitioner-workflows-and-commentary-2026-09.md#published-practitioner-workflows) — approaches to planning, context, implementation, and review.

[Back to learning](../README.md)
