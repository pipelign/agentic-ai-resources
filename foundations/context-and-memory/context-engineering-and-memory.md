# Context Engineering and Memory

**Greg Van Aken: September 15, 2026**

> AI Use Disclosure: Codex was used to research, organize, and draft this guide.

Useful agent behavior depends on which information is available at each step and how important state survives between steps or sessions. This article covers context selection, persistent state, compaction, and caching, with illustrative research examples and documented coding-agent implementations.

**Audience and prerequisites:** Readers designing or using agents for extended tasks. Start with [How agents work](../agent-loops-and-autonomy/how-agents-work.md) for the model, harness, and tool terminology.

## Contents

- [What context engineering means](#what-context-engineering-means)
- [Retrieval-augmented generation](#retrieval-augmented-generation)
- [State, memory, compaction, and caching](#state-memory-compaction-and-caching)
- [Illustrative example: continuing a research task](#illustrative-example-continuing-a-research-task)

## What context engineering means

*Context* is the information made available to the model for a particular inference. *Context engineering* is the work of selecting, organizing, retrieving, maintaining, and retiring that information across an agent’s execution.

Prompt writing is one part of it. In an extended agent task, the larger problem is deciding which instructions, documents, tool outputs, observations, and previous decisions the model should see at each step. Anthropic frames this as maintaining a useful, bounded information set as the agent repeatedly acts and receives new evidence. [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents).

### What can enter the context

| Kind of information | Typical contents | Common failure |
| --- | --- | --- |
| Runtime instructions | General behavior, tool-use rules, execution constraints. | Conflicting or overly broad instructions. |
| Workspace guidance | Research criteria, business procedures, repository conventions. | Stale guidance or excessive always-loaded material. |
| Task description | Desired outcome, scope, acceptance criteria. | Missing constraints or an ambiguous definition of success. |
| Conversation and working state | Decisions, recent observations, current plan. | Important facts become buried or are lost during summarization. |
| Tool descriptions | Names, argument schemas, usage guidance. | Too many overlapping capabilities or unclear interfaces. |
| Retrieved material | Articles, business records, source files, documentation, logs. | Irrelevant excerpts, missing dependencies, or misleading external instructions. |
| Skill material | Catalog descriptions, activated instructions, selected references. | Wrong skill selection or unnecessary resource loading. |
| Multimodal observations | Screenshots, rendered pages, diagrams. | Producing an artifact without actually supplying it to a model that can inspect it. |

The **context window** is the model's bounded capacity for context, usually measured in tokens. Instructions, conversation history, retrieved material, and generated output must fit within the applicable limits. As a task grows, the harness may need to select fewer excerpts, remove old output, or compact the history. A larger window permits more material; usefulness still depends on relevance and how well the model uses it. [Context engineering and its limits](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents).

A document collection or code repository can contain thousands of files while a particular call sees only a few excerpts. An agent may search and open additional files, but their existence on disk does not mean the model has already read them. Similarly, a screenshot saved by a tool is not necessarily a screenshot the model has inspected.

### How useful context gets selected

Common approaches combine predictable initial guidance with retrieval during the task. Workspace instructions describe the task environment and its conventions. Searches, file reads, record lookups, and documentation tools supply details when needed. Context engineering covers the selection, timing, presentation, and subsequent handling of that material.

For example, a failed test may produce a large log. Useful context might be the failure summary, the relevant stack frames, and the changed function. Loading the entire log can consume space without improving diagnosis. Conversely, trimming away the actual assertion can remove the only useful evidence. The aim is sufficient, relevant information, not simply the fewest tokens.

Persistent instruction files are one mechanism for supplying initial context. `AGENTS.md` describes an open convention for repository guidance; exact discovery and precedence depend on the host. Product-specific mechanisms also exist. These files commonly identify commands, conventions, and constraints that would otherwise need rediscovery. [AGENTS.md](https://agents.md/).

Claude Code explicitly distinguishes instructions loaded as context from enforced configuration. Its project guidance can also be scoped by directory or file patterns. This makes a useful general distinction: an instruction can influence a model’s choice, while an execution policy can prevent an operation regardless of that choice. [Claude Code memory and project instructions](https://code.claude.com/docs/en/memory).

A practical synthesis is to keep standing guidance focused, retrieve task-specific details on demand, preserve the source of important claims, and revisit originals when a summary is insufficient. This requires both harness features and judgment during the task.

For an illustrative research task, useful context might include the question, inclusion criteria, selected passages, and the source of each claim. A short summary that drops a study's limitations could lead to a misleading comparison. Choosing context includes deciding which qualifications must travel with the evidence.

## Retrieval-augmented generation

**Retrieval-augmented generation (RAG)** combines finding relevant material with generating a response that uses it. Retrieval supplies evidence; generation produces the answer. The [original RAG paper](https://arxiv.org/abs/2005.11401) combined a document retriever with a pretrained language generator. Its particular architecture is one implementation of that combination.

Consider an illustrative request: “Can this delayed order be refunded?”

| Step | What happens |
| --- | --- |
| Find material | Search the policy collection for delivery and refund rules. |
| Select passages | Keep the applicable policy sections and their source references. |
| Supply context | Give the model the question, order details, and selected passages. |
| Generate | Draft an answer using the supplied information. |
| Check | Verify that the cited policy supports the answer and applies to this order. |

An [embedding-based search](../neural-networks/neural-networks-embeddings-and-language-models.md#5-sentence-embeddings-and-semantic-search) can help find passages with related meaning. Keyword searches and direct lookups can also supply useful context. The model receives the selected results; it does not automatically see the whole collection.

Retrieval can fail to find an exception, and generation can misinterpret a passage that was found. Those are separate problems to diagnose. In the example, a refund answer should account for both the order's circumstances and the applicable rule.

Supplying a policy in context lets a trained model use it for this request. Updating the model's weights would require training. Retrieved text also remains source material: it does not grant permission to issue a refund or change the task. See [instructions, evidence, and permissions](../agent-loops-and-autonomy/how-agents-work.md#instructions-evidence-and-permissions).

## State, memory, compaction, and caching

An agent can appear to “remember” through several mechanisms that should be distinguished.

| Mechanism | What persists or changes | What it does not imply |
| --- | --- | --- |
| Conversation state | Messages, tool calls, and results retained by the application or API. | That every historical item remains in the active context forever. |
| Files and external state | Reports, plans, notes, code, commits, process state, or database records. | That the model sees those contents without retrieval. |
| Managed memory | Selected information stored and reintroduced by the application. | A change to the model’s learned weights. |
| Compaction | A shorter representation replaces or supplements older context. | Perfect preservation of every detail. |
| Prompt caching | Reuse of computation for a matching input prefix. | Semantic memory of information absent from the input. |

### Conversation state and durable artifacts

An API can preserve continuity through conversation objects or references to previous responses. The application may therefore manage a continuing conversation without literally resending every message in each request. This is an implementation detail separate from the question of which information the model ultimately receives. [OpenAI conversation state](https://developers.openai.com/api/docs/guides/conversation-state).

Files provide another form of continuity. A later session can inspect a plan, commit history, or recorded test result even after the original conversation has ended. For long-running work, Anthropic describes an initializer that prepares the environment and subsequent coding sessions that use progress records and incremental changes. The essential mechanism is external state that a new context can inspect. [Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents).

### Compaction

As a conversation grows, a harness may remove older outputs or summarize earlier work. This frees context capacity but can lose details such as a rejected alternative or an exact constraint. A useful continuation record identifies the goal, decisions, unfinished work, important evidence, and where the originals can be found. A summary should be treated as a navigation aid when exact details matter. Anthropic discusses compaction, structured notes, and delegated contexts as complementary approaches. [Context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents).

Skills create a related lifecycle problem: their instructions can become ineffective if discarded with old tool output. Integration guidance recommends preserving active skill content and avoiding duplicate injection. Actual retention behavior is host-specific. [Skill context management](https://agentskills.io/client-implementation/adding-skills-support).

### Prompt caching

Prompt caching reuses computation associated with a matching prompt prefix. Stable instructions and tool definitions can make repeated requests cheaper or faster. It does not retrieve forgotten facts, enlarge the context window, or make the output deterministic. Changes to the relevant prefix affect reuse; model generation still operates on the supplied context. [OpenAI prompt caching](https://developers.openai.com/api/docs/guides/prompt-caching).

These distinctions explain a common apparent contradiction: a system can retain a complete transcript, have cached earlier computation, and still fail to use an old detail. Storage, cache reuse, and active contextual availability are separate properties.

## Illustrative example: continuing a research task

Suppose a literature review needs several sessions. A continuation record could contain:

- The research question and criteria for including sources.
- A source list with links, extracted claims, and limitations.
- Decisions already made, including reasons for excluding material.
- Unresolved disagreements and the next documents to inspect.
- The location of the working comparison and draft report.

A new session would retrieve the record and open the evidence needed for its next step. If a summary omitted a study's participant count, the agent would need to return to the source. Compaction could shorten the working history; prompt caching would only concern reuse of computation for matching input. Neither supplies a missing fact by itself.

This invented example connects persistent records to the active information needed for a particular decision.

## Related reading

- [Skills, tools, hooks, and delegation](../tool-use-and-integrations/skills-tools-hooks-and-delegation.md) covers loading procedures and delegating work to separate contexts.
- [Agentic coding practices](../../use-cases/software-development/agentic-coding-best-practices-2026-09.md) applies these concepts to software work.
- [Topic index and further reading](README.md) lists supporting material.
