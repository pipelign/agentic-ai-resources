# Tool Use and Integrations

How agents discover procedures, access capabilities, respond to events, and delegate work. This topic includes the interaction between these mechanisms and the host that implements them.

## Start here

- [Skills, tools, hooks, and delegation](skills-tools-hooks-and-delegation.md) starts with the basic tool exchange, then introduces optional integrations and procedures. Implementation sections assume basic familiarity with files and APIs.

## Specifications and integration guides

These primary sources are useful for readers packaging skills or implementing agent integrations.

| Material | Why read it |
| --- | --- |
| [Agent Skills specification](https://agentskills.io/specification) | The packaging format and progressive disclosure model. |
| [Adding skills support](https://agentskills.io/client-implementation/adding-skills-support) | How a host discovers, advertises, activates, and retains skills. |
| [MCP architecture](https://modelcontextprotocol.io/docs/2026-07-28/learn/architecture) | How hosts and servers expose tools and contextual resources. |

## Example skill packages

The provider-maintained repository below offers packages to inspect when learning how skills are structured. Check an individual package's host and runtime requirements before trying its examples.

| Project | What it provides | What to inspect |
| --- | --- | --- |
| [anthropics/skills](https://github.com/anthropics/skills) | Example skill packages with instructions and supporting resources. | How procedures, references, and scripts are divided. |

## Related topics

- [Context and memory](../context-and-memory/README.md)
- [A skill in a coding workflow](../../use-cases/software-development/agentic-coding-best-practices-2026-09.md#a-worked-example-fixing-a-browser-interface)
- [Development workflow packages](../../use-cases/software-development/README.md#repositories-and-reusable-workflow-packages)
- [All foundations](../README.md)

## Resource annotation credit

**Greg Van Aken: September 15, 2026**

> AI Use Disclosure: Codex was used to research, organize, and draft this guide.

The date above records the original annotations. Product details and repository contents can change.
