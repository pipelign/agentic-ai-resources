# Understanding Coding Agents

> AI Use Disclosure: Codex was used to research, organize, and draft this guide.

A reading and viewing path for understanding what happens during an agent's work: what it can see, how it acts, what feedback it receives, and where human judgment enters the process.

**Background:** Familiarity with reading code, repositories, tests, and reviewing changes. The [ML fundamentals path](ml-fundamentals.md) is optional background if you also want to understand the models underneath.

## 1. Picture the whole system

Start with [How agents work](../../foundations/agent-loops-and-autonomy/how-agents-work.md).

The component table separates the model, runtime, tools, and environment. The agent-loop diagram then connects them: a requested action produces an observation that informs the next step.

## 2. Understand what the agent can see

Read [What context engineering means](../../foundations/context-and-memory/context-engineering-and-memory.md#what-context-engineering-means), followed by [State, memory, compaction, and caching](../../foundations/context-and-memory/context-engineering-and-memory.md#state-memory-compaction-and-caching).

These explain the difference between information stored in a project and information available to the model at a particular moment. They also explain how decisions and working state can carry across a long task.

## 3. Connect procedures to actions

Read [Skills, tools, hooks, and delegation](../../foundations/tool-use-and-integrations/skills-tools-hooks-and-delegation.md).

This explains how a skill supplies a procedure, a tool performs an operation, a hook responds to an event, and a delegated agent works through a subtask. The host-specific examples show why the surrounding runtime matters.

## 4. Follow one task from request to result

Read [A worked example: fixing a browser interface](../../use-cases/software-development/agentic-coding-best-practices-2026-09.md#a-worked-example-fixing-a-browser-interface).

The example traces skill selection, investigation, reproduction, editing, and verification. It brings the preceding concepts together in one concrete task and identifies what evidence is available at each stage.

## 5. Understand feedback and completion

Continue through these sections of the coding guide:

- [Harness engineering and executable feedback](../../use-cases/software-development/agentic-coding-best-practices-2026-09.md#harness-engineering-and-executable-feedback) — how the environment gives an agent useful evidence about its work.
- [Current working practices](../../use-cases/software-development/agentic-coding-best-practices-2026-09.md#current-working-practices) — task clarity, context selection, incremental changes, and review.
- [What the evidence does and does not establish](../../use-cases/software-development/agentic-coding-best-practices-2026-09.md#what-the-evidence-does-and-does-not-establish) — how to interpret claims about workflow effectiveness and productivity.

## 6. Bring human understanding into the picture

Use these annotated videos as a viewing sequence:

1. [Geoffrey Litt: understanding as participation](../../use-cases/software-development/practitioner-workflows-and-commentary-2026-09.md#understanding-as-participation-geoffrey-litt) — why understanding a change matters for deciding what comes next.
2. [Matt Pocock: a complete development workflow](../../use-cases/software-development/practitioner-workflows-and-commentary-2026-09.md#a-complete-development-workflow-matt-pocock) — how clarification, implementation, and review connect in practice.
3. [Dex Horthy: keeping teams aligned in complex codebases](../../use-cases/software-development/practitioner-workflows-and-commentary-2026-09.md#keeping-teams-aligned-in-complex-codebases-dex-horthy) — how research, plans, and context management support shared understanding.

The [full video collection](../../use-cases/software-development/practitioner-workflows-and-commentary-2026-09.md#videos-improving-quality-and-human-understanding) adds software fundamentals, testing, and continuous feedback.

## Explore different working styles

Finish with [Published practitioner workflows](../../use-cases/software-development/practitioner-workflows-and-commentary-2026-09.md#published-practitioner-workflows). The comparison shows how practitioners arrange planning, context, implementation, and review around different tasks and preferences.

[Back to learning](../README.md)
