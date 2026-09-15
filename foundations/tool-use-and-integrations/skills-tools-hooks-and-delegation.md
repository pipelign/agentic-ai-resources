# Skills, Tools, Hooks, and Delegation

**Greg Van Aken: September 15, 2026**

> AI Use Disclosure: Codex was used to research, organize, and draft this guide.

Skills describe procedures, tools expose operations, hooks respond to events, and delegated agents carry out subtasks. This article explains their roles and how they interact. Named host behaviors retain the September 2026 source context; research and support scenarios are illustrative.

**Audience and prerequisites:** Readers configuring or building agent workflows. Familiarity with the [agent loop](../agent-loops-and-autonomy/how-agents-work.md) is useful; implementation sections assume basic familiarity with files and APIs.

## Contents

- [How a harness discovers and uses a skill](#how-a-harness-discovers-and-uses-a-skill)
- [Tools, MCP, hooks, and subagents](#tools-mcp-hooks-and-subagents)
- [Illustrative example: customer-support triage](#illustrative-example-customer-support-triage)

## How a harness discovers and uses a skill

A skill is usually a directory containing a `SKILL.md` file and optional supporting files. The Agent Skills specification defines metadata such as the skill’s name and description, followed by Markdown instructions. References, scripts, and assets can accompany those instructions. The format supports *progressive disclosure*: make a small amount of discovery information available first, then load more only when relevant. [Agent Skills specification](https://agentskills.io/specification).

| Part of an illustrative skill | Purpose |
| --- | --- |
| `SKILL.md` metadata | Identifies the skill and describes when it is useful. |
| `SKILL.md` body | Explains the procedure and how to use its resources. |
| `references/source-checklist.md` | Provides detailed guidance for checking research sources. |
| `scripts/format-source-list.py` | Implements a repeatable operation outside the model. |
| `assets/report-template.md` | Provides material to reuse in an output. |

### Discovery and activation

A common lifecycle is:

1. **Discover.** The host finds installed skills in configured locations and reads their metadata.
2. **Advertise.** It exposes a catalog of names, descriptions, and a way to access each skill. This can be part of the model’s context or a tool description.
3. **Select.** The model identifies a relevant skill from the task and catalog, or the user explicitly invokes one.
4. **Activate.** The full instructions enter the conversation through a file read, a dedicated activation tool, or direct host injection.
5. **Follow and retrieve.** The agent applies the procedure and accesses supporting resources as needed.

The Agent Skills integration guide describes both ordinary file-read activation and dedicated activation tools. It also describes explicit invocation handled by the host. Selection is commonly the model’s semantic judgment, rather than a hard-coded keyword match. There is no universal `Skill()` API shared by all clients. [Adding skills support to an agent](https://agentskills.io/client-implementation/adding-skills-support).

### What actually changes when a skill loads

The model receives additional instructions. Its weights are not updated, and the Markdown does not become executable code merely by entering context. In the ordinary case, the same agent continues working with a more specific procedure available.

Supporting files enter context separately. A reference document contributes text when read. A script can execute without its source code being loaded into the prompt; the tool’s reported output supplies the observation instead. An asset can be copied or transformed as a file without all of its contents being represented in the conversation. Anthropic’s skill documentation explicitly separates metadata, instructions, and resources into these loading levels. [Agent Skills overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview).

For executable helpers, the agent still needs an appropriate tool and an environment with the necessary runtime and dependencies. Clear arguments, useful errors, meaningful exit codes, and predictable outputs make these helpers easier to operate. Paths must resolve against the correct skill directory rather than accidentally against the project checkout. [Using scripts in skills](https://agentskills.io/skill-creation/using-scripts).

### What varies between hosts

The open packaging convention does not specify identical runtime behavior everywhere. Hosts differ in installation paths, catalog limits, invocation syntax, compaction behavior, and supported metadata.

Some metadata is operational configuration. For example, Claude Code supports fields that affect tool access, request a separate subagent context, or register hooks. Its `allowed-tools` behavior can preapprove tools during a skill invocation. Consequently, a skill package can contain both model-facing prose and host-interpreted settings. Reviewing only the Markdown body misses part of its behavior. These controls are product-specific; they should not be assumed to work in another client. [Claude Code skills](https://code.claude.com/docs/en/skills).

The distinction is therefore: **the model interprets the procedure; the harness implements loading, execution, and supported configuration semantics.** A skill is not inherently a separate agent, an installed model capability, or a guaranteed permission boundary.

## Tools, MCP, hooks, and subagents

These mechanisms often appear together, but they solve different problems.

### Tools and MCP: access to capabilities

A tool interface tells the model what operation is available and which arguments it accepts. The implementation might execute a shell command, read a file, or call a remote service. Good interfaces expose a clear purpose and return enough information to choose the next action.

The **Model Context Protocol**, or MCP, standardizes communication between an AI application and servers exposing capabilities. Its architecture includes hosts, clients, and servers. Servers can expose tools, resources, and prompts: callable operations, contextual data, and reusable interaction templates. The host remains responsible for integrating those capabilities into its agent experience. MCP does not itself choose the task strategy or establish that a task is complete. [MCP architecture](https://modelcontextprotocol.io/docs/2026-07-28/learn/architecture).

A skill could instruct an agent to inspect an issue through an MCP tool, use local shell tools to make the fix, and verify it in a browser. In that arrangement, the skill supplies a procedure, MCP supplies an integration interface, and the harness coordinates the calls. The procedure could also work with a different integration exposing equivalent capabilities.

### Hooks: behavior attached to events

A hook runs because an event occurs, such as session startup, a tool request, or completion of an edit. This differs from guidance that asks the model to remember to perform an action.

Hook systems can run commands, call endpoints, or invoke model-based evaluators. A script that checks a path can provide a predictable decision; a model-based hook adds another inference with its own limitations. Whether a hook can block an operation or merely observe it depends on its event and host semantics. Claude Code documents these distinctions through its event inputs and decision controls. [Hooks reference](https://code.claude.com/docs/en/hooks).

For a requirement such as “this check must pass before merge,” a protected CI gate provides a different enforcement point from a local instruction or a hook the developer can disable. The appropriate mechanism depends on what must be guaranteed and who controls the runtime.

### Subagents: additional contexts and execution loops

A subagent receives a delegated task and performs its own model-and-tool loop. This can isolate a lengthy investigation, enable parallel work, or give a specialist different tools. The parent usually receives a result or summary, rather than automatically absorbing the entire child transcript. What is inherited from the parent varies by implementation and delegation mode. [Claude Code subagents](https://code.claude.com/docs/en/sub-agents).

Context isolation is not filesystem isolation. Two agents may have separate conversations while modifying the same files. Separate worktrees can isolate edits, but databases, ports, remote services, and credentials may still be shared. Useful delegation therefore needs both an information contract—what question to answer and what evidence to return—and an execution boundary.

For an illustrative research workflow, one delegated agent could examine methods while another examines reported results. Each would return source links, relevant passages, and unresolved questions. If both edit the same comparison table, their separate conversations would still leave a shared editing problem to coordinate.

Parallelism is most straightforward for independent investigations or changes with clear ownership. It becomes harder when multiple agents repeatedly alter the same interfaces. Cursor’s published scaling experiments describe coordination problems with an undifferentiated worker pool and a subsequent separation of planning and execution roles. Those are lessons from a particular large-scale experiment, not evidence that every coding task needs an agent hierarchy. [Scaling long-running autonomous coding](https://cursor.com/blog/scaling-agents).

## Illustrative example: customer-support triage

Suppose an agent is asked to prepare a draft response to a delivery complaint. In a hypothetical configuration:

| Mechanism | Role in the task |
| --- | --- |
| Skill | Describes how to classify the complaint, gather evidence, and prepare a response. |
| Tool | Retrieves the order status or saves the response draft. |
| MCP connection | Exposes the support system's capabilities to the host, if that system provides an MCP integration. |
| Hook | Records a tool result when the host emits the relevant event. |
| Delegated agent | Investigates a policy question and returns the applicable source and any uncertainty. |
| Harness | Loads the selected instructions, executes allowed operations, and passes results back to the model. |

The skill would guide the work, while the available tools determine which records the agent can inspect or change. The host's implementation determines whether the hook and delegation behavior described here is available.

## Related reading

- [Context engineering and memory](../context-and-memory/context-engineering-and-memory.md) explains instruction retention and the limits of summaries.
- [The browser-interface worked example](../../use-cases/software-development/agentic-coding-best-practices-2026-09.md#a-worked-example-fixing-a-browser-interface) traces a skill through a coding task.
- [Topic index and further reading](README.md) includes specifications and example skill packages.
