# Agentic Coding: Practices and Evidence

**Greg Van Aken: September 15, 2026**

> AI Use Disclosure: Codex was used to research, organize, and draft this guide.

Agentic coding combines a language model with a runtime that lets it inspect a project, execute tools, change files, observe results, and continue working. Its effectiveness depends on the interaction between the model, the information available to it, the tools it can operate, and the feedback that tells it whether it succeeded.

This guide connects a worked example to engineering practices and evidence available in September 2026. Product documentation describes supported behavior; practitioner accounts illustrate particular approaches; empirical results apply to their studied settings. Recommendations and illustrative examples are a synthesis, not a claim that one workflow has been universally validated.

**Audience and prerequisites:** Developers and technical reviewers familiar with repositories, tests, and code review. The [software-development reading path](README.md#suggested-reading-order) introduces the agent concepts used here.

## Contents

- [A worked example: fixing a browser interface](#a-worked-example-fixing-a-browser-interface)
- [Harness engineering and executable feedback](#harness-engineering-and-executable-feedback)
- [Current working practices](#current-working-practices)
- [What the evidence does and does not establish](#what-the-evidence-does-and-does-not-establish)

## A worked example: fixing a browser interface

The following is an invented example illustrating the [skill lifecycle and tool execution](../../foundations/tool-use-and-integrations/skills-tools-hooks-and-delegation.md). It is not a transcript of an actual run, and the paths and helper names are hypothetical.

**Task:** “The account menu closes before a keyboard user can select an item. Fix it and verify keyboard behavior.”

Assume the harness has file, shell, and browser tools. It discovers a skill called `verify-web-interaction`. The skill’s core file might contain:

```markdown
---
name: verify-web-interaction
description: Verify browser interaction changes, including keyboard and focus behavior.
---

# Verify a web interaction

1. Identify the expected interaction and reproduce the reported failure.
2. Read references/keyboard-checks.md when focus or keyboard input is involved.
3. Inspect the implementation and make the smallest coherent correction.
4. Run the relevant automated checks and exercise the interaction in a browser.
5. Report the observed behavior, evidence, and any verification gaps.

Use scripts/capture-page.mjs when a screenshot would clarify the result.
Resolve bundled paths from this skill's directory.
```

The resulting execution could look like this:

| Stage | Harness or tool activity | What the model now has available |
| --- | --- | --- |
| 1. Start | Load project instructions, the task, tool interfaces, and the skill catalog. | Commands and constraints; a short skill description. No automatic access to every source file or skill resource. |
| 2. Select | Receive the model’s request to load `verify-web-interaction`. | The task and catalog were sufficient to choose the procedure. |
| 3. Activate | Read the skill and return its instructions with the resource location. | The full procedure, alongside the existing conversation. |
| 4. Retrieve | Read the keyboard reference and search for the account-menu implementation. | Specific interaction checks and relevant code. |
| 5. Reproduce | Start the application and use browser tools to focus and operate the menu. | Observed behavior, such as focus leaving the menu unexpectedly. |
| 6. Edit | Apply the requested patch to the actual checkout. | A patch result; the agent can inspect the resulting diff. |
| 7. Check | Run the relevant tests and browser interaction again. | Passes, failures, console output, and fresh behavioral observations. |
| 8. Inspect evidence | If useful, run the capture helper, then load its screenshot through an image-capable tool. | The screenshot itself, rather than merely a path to it. |
| 9. Iterate or finish | Continue if evidence shows a problem; otherwise prepare the result. | Enough observed evidence to explain the fix and its verification limits. |

Several details clarify the division of labor. The skill suggests a procedure; it does not directly press keys or edit the component. The browser and file tools perform those operations when the harness executes their calls. The screenshot helper produces an artifact; another operation may be necessary to make that image visible to the model. The model evaluates the observations and chooses what to do next.

Failures remain possible at every layer. The skill may not be selected. A stale instruction may name the wrong command. The application may fail to start. The model may accept an inadequate test. Diagnosing these separately is more useful than treating every failure as a limitation of the model’s coding ability.

For this example, a meaningful completion report would identify the behavior changed, the checks actually performed, and any remaining gap. “Tests pass” would be incomplete if the agent never exercised the keyboard interaction that motivated the task.

## Harness engineering and executable feedback

*Harness engineering* includes the work of making an agent’s operating environment reliable and informative. At the product level, this involves tool execution, permissions, context, and recovery. At the project level, it involves setup, documentation, commands, fixtures, and checks. Birgitta Böckeler distinguishes these builder and user perspectives and emphasizes both guidance before action and feedback afterward. [Harness engineering for coding agent users](https://martinfowler.com/articles/harness-engineering.html).

The feedback portion is especially important. A model can produce plausible code from text alone. Execution supplies observations that can contradict its assumptions.

| Feedback mechanism | What it can establish | Important limit |
| --- | --- | --- |
| Formatting, linting, and type checks | Compliance with encoded structural rules. | They do not establish that the requested behavior is correct. |
| Focused regression tests | The specific tested failure is caught and corrected. | A test can encode the wrong expectation or miss adjacent cases. |
| Integration tests | Selected components work together under tested conditions. | Fixtures may omit production behavior. |
| Browser or manual execution | The actual interface or workflow behaves as observed. | Observed examples are not exhaustive coverage. |
| Diff and architectural review | The change fits constraints, scope, and design intent. | Review can miss defects; generated review has its own blind spots. |

Simon Willison’s red/green testing pattern makes feedback concrete: establish that a test detects the missing behavior, implement the change, then observe it pass. His manual-testing guidance adds a complementary point: agents should run and exercise the software they change, including behavior that automated tests do not cover. [Red/green TDD](https://simonwillison.net/guides/agentic-engineering-patterns/red-green-tdd/), [Agentic manual testing](https://simonwillison.net/guides/agentic-engineering-patterns/agentic-manual-testing/).

A useful environment makes failures interpretable. A test command should identify the failing assertion; a browser workflow should expose application errors; a setup command should fail clearly when prerequisites are missing. This allows the next model step to be grounded in evidence instead of another guess.

Permissions form another part of the harness. Tool restrictions, filesystem boundaries, network controls, and approval rules govern which proposed actions can execute. Instructions such as “do not expose secrets” influence behavior, but enforcement requires controls outside the model. Anthropic’s sandboxing account illustrates the use of filesystem and network isolation to constrain execution. [Claude Code sandboxing](https://www.anthropic.com/engineering/claude-code-sandboxing).

This also matters when reading external material. Repository text, issue descriptions, webpages, and tool output may contain instructions that are unrelated to the user’s task. Preserving their provenance and limiting available authority helps prevent retrieved content from being treated as permission to perform a new action. Installing a skill or connector therefore changes the system’s inputs or capabilities and merits the same scrutiny as other executable development tooling.

## Current working practices

The most consistent practices across current guidance are relatively straightforward. They aim to make intent clear, reduce avoidable uncertainty, and provide observable feedback.

### Define a verifiable task

Specify the desired outcome, relevant context, constraints, and what would demonstrate completion. An issue description that identifies a reproducible bug or a bounded feature gives an agent a firmer target than an open-ended request to improve a system. OpenAI emphasizes context and clear outcomes; GitHub similarly recommends well-scoped tasks and explicit acceptance criteria for its cloud agent. [OpenAI best practices](https://learn.chatgpt.com/guides/best-practices), [GitHub cloud-agent guidance](https://docs.github.com/en/copilot/tutorials/cloud-agent/get-the-best-results).

### Scale planning to uncertainty

An unfamiliar architecture or cross-cutting change benefits from investigation and a reviewed plan. A small, well-understood edit may not need a separate planning artifact. Claude Code’s guidance describes exploration, planning, implementation, and verification while acknowledging that planning adds overhead for simple work. [Claude Code best practices](https://code.claude.com/docs/en/best-practices).

The useful decision is where a mistaken assumption would be expensive. Reviewing that assumption early is usually more valuable than inspecting every routine action afterward.

### Keep instructions and skills selective

Standing rules should contain durable, applicable guidance. Specialized procedures can be loaded when relevant, and detailed references can stay outside the active context until needed. Cursor recommends focused rules, concrete examples, and on-demand skills. [Cursor agent practices](https://cursor.com/blog/agent-best-practices).

A skill should solve a recurring failure or encode a useful procedure. Evaluate both **selection**—whether it activates for appropriate tasks—and **execution**—whether its use improves the result. Compare against a baseline without the skill, include realistic cases, and repeat evaluations when the model or environment changes. Anthropic’s authoring guidance and skill-creator work emphasize evaluation and iteration; some skills encode lasting preferences, while others compensate for limitations that later models may no longer have. [Skill authoring practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices), [Testing and refining skills](https://claude.com/blog/improving-skill-creator-test-measure-and-refine-agent-skills).

### Work in reviewable increments

Smaller coherent changes make it easier to connect a requirement, implementation, and test result. Incremental commits or checkpoints also help recover after an unsuccessful direction. Long-running systems need explicit progress and completion criteria so a later session can distinguish finished work from an optimistic earlier statement. [Long-running agent harnesses](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents).

### Verify the relevant behavior and inspect the change

Choose checks that could detect a meaningful error in this task. Then review the diff for unintended changes, weakened tests, unnecessary dependencies, and design implications. A generated review can assist triage, but it should be evaluated as another fallible analysis. Addy Osmani’s review workflow emphasizes directing attention according to the risk of the change. [Agentic code review](https://addyosmani.com/blog/agentic-code-review/).

### Use autonomy where feedback is strong

The synthesis from these practices is that task clarity and feedback quality are useful guides to autonomy. A bounded change with a reliable reproduction and fast tests gives the system a clear correction loop. An ambiguous redesign with weak checks leaves more of the judgment unresolved. Adding more agents or longer execution time does not by itself supply that missing judgment.

The [video guide](practitioner-workflows-and-commentary-2026-09.md#videos-improving-quality-and-human-understanding) develops the human side of this transition: strengthening understanding, turning expectations into executable checks, and using faster implementation to make room for better design and review.

## What the evidence does and does not establish

There is substantial evidence that coding agents can complete meaningful software tasks. There is less agreement about a universal best configuration or a single productivity multiplier.

Research on repository instruction files illustrates the uncertainty. Gloaguen and colleagues found that additional context files did not generally improve success in their evaluated settings and could increase cost. Lulla and colleagues reported lower runtime and token use with `AGENTS.md` in a different setup. These results concern different tasks, instructions, and evaluations; neither establishes that all instruction files help or all should be removed. [Gloaguen et al., revised June 2026](https://arxiv.org/html/2602.11988v2), [Lulla et al., revised March 2026](https://arxiv.org/html/2601.20404v2).

METR’s February 2026 update also cautions against simple extrapolation. Its earlier experiment found experienced developers took longer with the studied tools. Later measurements suggested improvement but had selection and measurement issues that limited a clean comparison. A May survey reported perceived gains among technical workers, but self-reported work value is not a randomized measure of coding productivity. [METR experiment update](https://metr.org/blog/2026-02-24-uplift-update/), [METR usage survey](https://metr.org/blog/2026-05-11-ai-usage-survey/).

For evaluating a workflow, useful questions include:

- Did the change meet the requirement and survive review?
- How much human attention, rework, elapsed time, and compute did it require?
- Which tasks failed or were abandoned, rather than merely which succeeded?
- Did a skill, extra context, or another agent improve the outcome relative to a simpler baseline?
- Does the result persist across representative tasks and model updates?

These questions favor measuring completed engineering outcomes over generated lines, tool-call counts, or the apparent confidence of an agent’s final response.

## Related reading

- [Practitioner workflows and commentary](practitioner-workflows-and-commentary-2026-09.md) collects published approaches and annotated videos.
- [Software-development index and further reading](README.md) includes workflow packages and the full reading path.
