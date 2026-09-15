# Software Development

Guides and resources for using agents to investigate, implement, test, and review software changes. The coding practices and practitioner collection describe the state of practice in September 2026.

## Guides

- [Agentic coding: practices and evidence](agentic-coding-best-practices-2026-09.md) — a browser-interface worked example, harness engineering, working practices, and the limits of current evidence.
- [Practitioner workflows and commentary](practitioner-workflows-and-commentary-2026-09.md) — published approaches and annotated videos about quality, understanding, and review.

Both guides assume familiarity with repositories, tests, and code review.

## Suggested reading order

1. [How agents work](../../foundations/agent-loops-and-autonomy/how-agents-work.md) — understand the model, harness, tools, environment, and action loop.
2. [Context engineering and memory](../../foundations/context-and-memory/context-engineering-and-memory.md) — understand what information reaches the model and survives between steps.
3. [Skills, tools, hooks, and delegation](../../foundations/tool-use-and-integrations/skills-tools-hooks-and-delegation.md) — connect procedures, integrations, and separate agent contexts.
4. [Agentic coding practices](agentic-coding-best-practices-2026-09.md) — follow the worked example and examine engineering feedback and evidence.
5. [Practitioner workflows and commentary](practitioner-workflows-and-commentary-2026-09.md) — compare published approaches and explore the accompanying talks.

## Further reading

This practitioner explanation is useful for developers improving the environment and feedback available to a coding agent.

| Material | Why read it |
| --- | --- |
| [Harness engineering for coding agent users](https://martinfowler.com/articles/harness-engineering.html) | How repository environments and feedback support the runtime. |

## Repositories and reusable workflow packages

These projects describe different development workflows. The annotations identify what to examine; suitability depends on the task, host, and package requirements. They are useful to developers familiar with the practices described in the guides above.

| Project | What it provides | What to inspect |
| --- | --- | --- |
| [mattpocock/skills](https://github.com/mattpocock/skills) | Composable skills accompanying the AI Hero engineering workflow. | How clarification, specifications, task decomposition, TDD, implementation, and review share procedures and pass context between stages. |
| [obra/superpowers](https://github.com/obra/superpowers) | A collection of development skills and an opinionated workflow. | How planning, implementation, testing, and review are connected. |
| [github/spec-kit](https://github.com/github/spec-kit) | A toolkit for specification-driven development with coding agents. | The artifacts passed between specification, planning, and implementation. |
| [EveryInc/compound-engineering-plugin](https://github.com/EveryInc/compound-engineering-plugin) | A plugin organized around planning, work, review, and recording reusable learning. | The relationship between commands, skills, agents, and stored knowledge. |
| [humanlayer/advanced-context-engineering-for-coding-agents](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents) | Materials accompanying a research, planning, and implementation approach. | How intermediate documents structure subsequent agent context. |

See the [tool-use index](../../foundations/tool-use-and-integrations/README.md) for the skill specifications and general example packages. The foundations indexes collect documentation for the agent loop and context engineering.

## Browse the collection

- [All use cases](../README.md)
- [Foundations](../../foundations/README.md)
- [Repository overview](../../README.md)

## Resource annotation credit

**Greg Van Aken: September 15, 2026**

> AI Use Disclosure: Codex was used to research, organize, and draft this guide.

The date above records the original annotations. Product details and repository contents can change.
