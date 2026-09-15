# Agentic Coding: Practitioner Workflows and Commentary

**Greg Van Aken: September 15, 2026**

> AI Use Disclosure: Codex was used to research, organize, and draft this guide.

This collection brings together published development workflows and annotated talks as of September 2026. It connects practical choices about planning, context, implementation, and review to the purposes they serve. Practitioner accounts describe particular settings; their results need to be evaluated in the context of your own work.

**Audience and prerequisites:** Developers and technical leads comparing ways to work with coding agents. Read [Agentic coding practices](agentic-coding-best-practices-2026-09.md) for the worked example and evidence discussion.

## Contents

- [Published practitioner workflows](#published-practitioner-workflows)
- [Videos: improving quality and human understanding](#videos-improving-quality-and-human-understanding)

## Published practitioner workflows

These accounts are useful examples of how people structure work. They describe different preferences and task settings; they are not interchangeable controlled experiments.

| Practitioner or team | Published approach | What it illustrates |
| --- | --- | --- |
| **Simon Willison** | Uses agentic engineering patterns centered on executing code, regression tests, and manual exercise of software. | Verification is an active part of implementation. See [Agentic Engineering Patterns](https://simonwillison.net/guides/agentic-engineering-patterns/). |
| **Addy Osmani** | Describes a workflow built around specifications, planning, relevant context, incremental implementation, and review. | Human effort can move toward clarifying intent and evaluating changes. See [AI coding workflow](https://addyosmani.com/blog/ai-coding-workflow/). |
| **Matt Pocock / AI Hero** | Publishes a development workflow as composable skills: clarify the idea, write a specification, divide work into tickets, implement, and review. | Connects engineering practices to concrete skill packages and deliberate context boundaries. See [AI Hero](https://www.aihero.dev/) and [his skills repository](https://github.com/mattpocock/skills). |
| **Boris Tane** | Separates research and planning into Markdown documents, annotates the plan, then permits implementation. | A persistent plan becomes a concrete interface for correcting assumptions before code changes. See [How I use Claude Code](https://boristane.com/blog/how-i-use-claude-code/). |
| **Dex Horthy / HumanLayer** | Uses research, planning, and implementation phases with deliberate context management. | Reviewed intermediate artifacts can compress investigation into useful inputs for later work. See [Advanced context engineering](https://www.humanlayer.dev/blog/advanced-context-engineering). |
| **Peter Steinberger** | Describes rapid iteration, direct tool use, concurrent work, and verification in a highly practiced personal workflow. | An experienced operator can choose different levels of involvement across tasks. His throughput is not a transferable baseline. See [Shipping at inference speed](https://steipete.me/posts/2025/shipping-at-inference-speed). |
| **Thariq Shihipar / Claude Code** | Describes skills used for recurring procedures, domain knowledge, gotchas, and operational workflows. | Skills can encode practical knowledge that would otherwise need rediscovery. See [How we use skills](https://www.linkedin.com/pulse/lessons-from-building-claude-code-how-we-use-skills-thariq-shihipar-iclmc). |
| **Geoffrey Huntley** | Popularized the Ralph loop: repeatedly run an agent against a bounded task, using external feedback and persistent state. | The outer loop and the quality of its stopping conditions matter alongside the individual model response. See [Ralph](https://ghuntley.com/ralph/). |

There are real differences between these approaches. A plan may remain in one long conversation or become the input to a fresh implementation context. Review can occur before implementation, at intermediate checkpoints, or mainly on the resulting diff. Parallel work may be central or absent. The transferable question is what each mechanism accomplishes: removing uncertainty, preserving decisions, keeping context focused, or obtaining evidence.

### Matt Pocock: a workflow expressed as skills

Pocock is particularly relevant to the connection between skills and everyday coding practice. His published main flow is `/grill-with-docs` → `/to-spec` → `/to-tickets` → `/implement` → `/code-review`. These are reusable procedures operated through an existing coding agent. [AI Hero workflow](https://www.aihero.dev/).

Four aspects stand out:

- **Clarification before implementation.** `/grill-with-docs` asks questions about the design and records resolved domain terminology and qualifying architectural decisions. It composes the `grilling` and `domain-modeling` skills; its documentation explicitly describes failures when those dependencies are not loaded. This is a concrete example of skill composition depending on correct instruction retrieval. [The clarification skill](https://www.aihero.dev/skills-grill-with-docs).
- **Tasks sized for fresh contexts.** `/to-tickets` aims to produce independently verifiable vertical slices, each small enough for a new agent session. Its “tracer bullet” approach favors a working path through the relevant layers. [The ticket-planning skill](https://www.aihero.dev/skills-to-tickets).
- **Behavior-driven feedback.** `/tdd` specifies incremental red/green development and tests at agreed public boundaries. It serves as a reference procedure that an implementation session follows. [The TDD skill](https://www.aihero.dev/skills-tdd).
- **An explicit implementation rhythm.** `/implement` works through a ticket with incremental tests and type checks, then a full test run and review. The intended pattern is one ticket per fresh context. [The implementation skill](https://www.aihero.dev/skills-implement).

This approach illustrates context engineering through the shape of the work: the specification and ticket carry information into the next session, while the skills supply repeatable procedures. It also allows a shorter path for small changes; the ticket-planning guidance says to skip decomposition when the change fits in one context. These are published workflow choices, not evidence that every task benefits from the full sequence. [Task-sizing guidance](https://www.aihero.dev/skills-to-tickets).

## Videos: improving quality and human understanding

These videos explore how development can change while improving correctness, maintainability, and human understanding. The selection favors concrete engineering practices and active human participation. Annotations draw on official video descriptions, publisher notes, and speakers’ written companions; the suggested applications are this report’s synthesis. The talks offer practices to evaluate, rather than controlled evidence that a particular workflow always raises quality.

### Software fundamentals: Robert C. Martin and Matt Pocock

**[LIVE: Uncle Bob on Software Fundamentals in the Age of AI](https://www.youtube.com/watch?v=zcLPGC-tvgk)** — Robert C. Martin, interviewed by Matt Pocock.

The interview examines why software fundamentals remain relevant when working with coding agents, including areas where the two practitioners agree and disagree. It provides a useful starting point for discussing how engineering responsibilities change as implementation becomes easier to delegate.

**Practical application:** Use the discussion to identify which standards a development workflow should make stronger: testability, understandable interfaces, manageable dependencies, or evidence of correct behavior. For each selected standard, identify how it will be checked and who remains responsible for judging the result.

### Understanding as participation: Geoffrey Litt

**[Understanding is the new bottleneck — Geoffrey Litt, Notion](https://www.youtube.com/watch?v=WkBPX-oDMnA)**.

Litt argues that understanding enables people to contribute to the next design iteration, beyond checking whether the current output is correct. He demonstrates code explainers, quizzes, and interactive “micro-worlds” that help developers build intuition about systems agents are changing. His [written version](https://www.geoffreylitt.com/2026/07/02/understanding-is-the-new-bottleneck) includes examples and a link to his `/explain-diff` skill.

**Practical application:** For a consequential change, request an explanation of the prior design, the reason for the change, and its important behavioral consequences. Add a small interactive example or a few comprehension questions where they would expose a misunderstanding. Check the explanation against the actual implementation; it is another generated artifact, not independent proof.

### A complete development workflow: Matt Pocock

**[Full Walkthrough: Workflow for AI Coding — Matt Pocock](https://www.youtube.com/watch?v=-QFHIoCo-Ko)** — AI Engineer.

This workshop connects clarification, product intent, vertical slices, agent execution, QA, and architectural decisions. Its value is seeing the handoffs between these activities and where human judgment remains necessary. The publisher’s [companion page](https://ai.engineer/talks/ai-coding-workflow) provides chapters and an explanation of the workflow.

Selected chapters, about **38 minutes** in total:

- [12:16–31:27](https://www.youtube.com/watch?v=-QFHIoCo-Ko&t=736s) — clarification, about **19 minutes**.
- [39:37–53:52](https://www.youtube.com/watch?v=-QFHIoCo-Ko&t=2377s) — vertical slices, about **14 minutes**.
- [1:09:04–1:13:54](https://www.youtube.com/watch?v=-QFHIoCo-Ko&t=4144s) — QA and review, about **5 minutes**.

The full workshop runs about **97 minutes**. Skill names and product details in the recording may differ from the packages described in the [Matt Pocock workflow section](#matt-pocock-a-workflow-expressed-as-skills).

**Practical application:** Try the workflow on one bounded feature. Resolve the important uncertainties, choose a slice with observable behavior, and explicitly review both the working result and the design it introduces. Adapt the amount of process to the task.

### Tests that challenge the implementation: Matt Pocock

**[My Skill Makes Claude Code GREAT At TDD](https://www.aihero.dev/skill-test-driven-development-claude-code)** — video and written companion on AI Hero.

Pocock describes a skill for incremental red/green/refactor development. He focuses on tests of observable behavior and the problems caused by tests that mostly exercise mocks or repeat implementation details. The example connects a reusable skill to an actual quality practice.

**Practical application:** Choose a bug with a clear reproduction. Have the agent demonstrate a failing regression test, make the correction, and show the same test passing. Inspect whether the test would catch the original defect and whether unrelated behavior still works. A test-first instruction helps structure execution; its actual use still needs observation.

### Executable expectations and continuous feedback: Dave Farley

**[Engineering Discipline in the AI Era with Dave Farley](https://www.aviator.co/podcast/engineering-discipline-dave-farley)** — The Hangar DX video interview and companion text.

Farley discusses test-driven and behavior-driven development, incremental learning, and verification of agent output. He challenges the idea that a complete specification can settle an exploratory development process in advance. The episode connects clear expectations with executable examples and ongoing feedback.

Watch **16:39–29:29**, about **13 minutes**, for the connected discussion of TDD and BDD, testing and feedback loops, and ambiguity in specifications.

**Practical application:** Turn an important user expectation into an executable acceptance example, then implement a small increment and inspect the result. Revise the next step using what was learned. Track whether failures are found earlier and whether the resulting tests remain useful as the design changes.

### Keeping teams aligned in complex codebases: Dex Horthy

**[No Vibes Allowed: Solving Hard Problems in Complex Codebases — Dex Horthy, HumanLayer](https://www.youtube.com/watch?v=rmvDxxNubIg)** — AI Engineer. The publisher also provides a [chaptered companion](https://ai.engineer/talks/context-engineering-for-complex-codebases).

Horthy connects context management with research, explicit plans, and human review. The emphasis on maintaining a shared understanding of the system makes this useful for existing codebases where plausible edits can conflict with architecture or hidden constraints. His reported outcomes describe his team’s experience, not a general productivity guarantee.

Selected chapters, about **8 minutes** in total:

- [7:41–10:11](https://www.youtube.com/watch?v=rmvDxxNubIg&t=461s) — research–plan–implement and an example.
- [10:11–12:07](https://www.youtube.com/watch?v=rmvDxxNubIg&t=611s) — responsibilities to retain.
- [15:02–18:23](https://www.youtube.com/watch?v=rmvDxxNubIg&t=902s) — mental alignment.

The full talk runs about **21 minutes**.

**Practical application:** Before a complex change, review a short account of the relevant code paths and proposed implementation. Correct mistaken assumptions while they are still inexpensive, then verify the resulting change against the agreed behavior.

### Applying the ideas to increase quality

A useful starting sequence is Litt for human participation, Martin and Pocock for fundamentals, and Pocock’s walkthrough for a concrete workflow. The testing and context-management videos then deepen specific parts of that workflow.

The common opportunity is to spend some of the implementation capacity gained from agents on work that previously received too little attention:

- **Better understanding:** clearer explanations, shared design discussions, and explicit checks of assumptions.
- **Better verification:** meaningful regression tests, executable acceptance examples, and inspection of the running software.
- **Better design:** comparison of alternatives, simpler interfaces, and incremental refactoring supported by tests.
- **Better learning:** small experiments followed by review of what actually happened.

Assess improvement through fewer escaped defects, less rework, stronger behavior coverage, and a team that can explain and safely change the system. Faster generation creates an opportunity for these gains; the development process determines whether that opportunity is used.

## Related reading

- [Evidence and its limits](agentic-coding-best-practices-2026-09.md#what-the-evidence-does-and-does-not-establish) discusses how to assess workflow claims.
- [Software-development index](README.md#repositories-and-reusable-workflow-packages) links to reusable packages.
- [Skills, tools, hooks, and delegation](../../foundations/tool-use-and-integrations/skills-tools-hooks-and-delegation.md) explains the mechanisms behind skill-based workflows.
