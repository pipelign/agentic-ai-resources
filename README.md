# Agentic AI Resources

> AI Use Disclosure: Codex was used to research, organize, and draft this guide.

A curated collection of explanations, tools, workflows, research, and practical examples for agentic AI across software development, research, personal productivity, business operations, creative work, and physical systems.

## Browse the collection

- [Foundations](foundations/README.md) — neural networks and embeddings, agent loops, context and memory, skills, tools, and delegation.
- [Use cases](use-cases/README.md) — applied guides and resources, starting with software development.

## Proposed directory structure

Organize resources primarily by subject and application. Keep articles, papers, videos, and repositories together when they help someone learn the same topic.

This is a proposed layout. Create directories as resources are added; each populated directory should have a `README.md` that explains its scope and indexes its contents.

```text
agentic-ai-resources/
├── README.md                         # Overview and navigation
├── CONTRIBUTING.md                   # Submission and curation guidelines
│
├── foundations/                      # Concepts shared across applications
│   ├── neural-networks/
│   ├── agent-loops-and-autonomy/
│   ├── planning-and-reasoning/
│   ├── context-and-memory/
│   ├── tool-use-and-integrations/
│   └── multi-agent-systems/
│
├── patterns/                         # Reusable ways to structure agent work
│   ├── research-and-synthesis/
│   ├── retrieval-and-knowledge/
│   ├── routing-and-delegation/
│   ├── human-in-the-loop/
│   ├── review-and-verification/
│   └── scheduled-and-event-driven/
│
├── tools-and-platforms/              # Product guides and comparisons
│   ├── models-and-providers/
│   ├── frameworks-and-runtimes/
│   ├── agent-apps/
│   ├── automation-platforms/
│   ├── browser-and-computer-use/
│   ├── protocols-and-connectors/
│   └── deployment-and-hosting/
│
├── use-cases/                        # Applied guides, workflows, and examples
│   ├── software-development/
│   ├── research-and-discovery/
│   ├── data-analysis/
│   ├── personal-productivity/
│   ├── business-operations/
│   ├── customer-support/
│   ├── sales-and-marketing/
│   ├── creative-and-media/
│   ├── education-and-training/
│   └── robotics-and-physical-systems/
│
├── evaluation-and-operations/        # Measuring quality and running agents
│   ├── task-success-and-benchmarks/
│   ├── testing-and-simulation/
│   ├── observability-and-debugging/
│   ├── reliability-and-recovery/
│   └── cost-and-performance/
│
├── safety-and-governance/            # Boundaries and accountability
│   ├── permissions-and-sandboxing/
│   ├── prompt-injection-and-security/
│   ├── privacy-and-data-handling/
│   └── oversight-and-auditability/
│
├── research/                        # Dated synthesis and original findings
│   ├── surveys/
│   ├── experiments/
│   └── open-questions/
│
├── learning/                        # Guided routes through the collection
│   ├── getting-started.md
│   ├── glossary.md
│   └── learning-paths/
│
├── templates/                       # Reusable starting points
│   ├── resource-entry.md
│   ├── workflow.md
│   ├── evaluation-plan.md
│   └── agent-instructions/
│
└── assets/                          # Images and diagrams used in documents
```

## Example resource placements

These examples illustrate how different resources could fit into the proposed structure.

| Example resource | Suggested primary home |
| --- | --- |
| Explanation of agent memory | `foundations/context-and-memory/` |
| General workflow for producing a sourced report | `patterns/research-and-synthesis/` |
| Guide to a specific agent framework | `tools-and-platforms/frameworks-and-runtimes/` |
| Workflow for managing an inbox or calendar | `use-cases/personal-productivity/` |
| Customer-support triage example | `use-cases/customer-support/` |
| Tutorial on evaluating task completion | `evaluation-and-operations/task-success-and-benchmarks/` |
| Guide to preventing prompt injection | `safety-and-governance/prompt-injection-and-security/` |
| Dated review of developments across agentic AI | `research/surveys/` |

Use one primary home for each resource and link to it from other relevant indexes. For example, a research paper about memory belongs in the memory index; `research/` holds substantial literature reviews, experiment reports, and discussions of open questions.

Within a use case, start with a `README.md` and individual guides. Add `workflows/` or `examples/` when there is enough material to justify them. A runnable example should document its setup, expected result, and required access.

## Curation conventions

- Use lowercase, hyphenated file and directory names.
- Date time-sensitive surveys and comparisons with `YYYY-MM` or `YYYY-MM-DD`; keep stable explanations undated.
- For each external resource, include a title, source link, short description, audience or prerequisites, and date last reviewed.
- Distinguish official documentation, research findings, practitioner accounts, and repository-authored recommendations.
- Keep vendor-specific setup in `tools-and-platforms/` and link to it from applied workflows.
- Prefer annotated selections over unfiltered link lists. Explain what a resource helps someone understand or accomplish.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for resource selection, contribution formats, and submission guidelines.
