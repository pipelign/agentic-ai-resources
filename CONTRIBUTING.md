# Contributing

> AI Use Disclosure: Codex was used to research, organize, and draft this guide.

Contributions are welcome across agentic AI: software development, research, personal productivity, business operations, creative work, education, robotics, and other applications.

Useful contributions include annotated resources, original explanations, practical workflows, runnable examples, corrections, and updates to outdated material.

## Choosing resources

- Explain the connection to agentic AI and what the resource helps someone understand or accomplish.
- Prefer resources with clear explanations, concrete examples, or evidence readers can inspect.
- Use primary sources for claims about product behavior, specifications, and research results. Practitioner accounts are useful when identified as such.
- Check for existing coverage. Improve an existing entry when the contribution addresses the same material.
- Describe strengths and limitations in plain language. Disclose your affiliation when submitting your own work or a product you represent.

## Where to put contributions

Follow the [directory structure and placement examples](README.md#proposed-directory-structure).

- Choose one primary home by subject or application, and link to it from other relevant indexes.
- Keep articles, papers, videos, and repositories about the same topic together.
- Put product-specific setup and comparisons in `tools-and-platforms/`, reusable workflows in `patterns/`, and application-specific guides in `use-cases/`.
- Use `research/` for literature reviews, experiment reports, and open questions. Individual papers can be listed in their subject's index.
- Create directories as content is added. Include a `README.md` explaining the scope and linking to the contents of each directory you populate.
- Use relative links for files in this repository.

If a contribution needs a new category, include a short explanation of its scope in the pull request.

## Adding an external resource

Add an annotated entry to the relevant directory's `README.md`. This format is a starting point:

```markdown
### [Resource title](https://example.com/resource)

- **Author or organization:** Name
- **Source type:** Official documentation / research / practitioner account
- **Format:** Article / paper / video / repository / course
- **Useful for:** What readers can learn or do, in one or two sentences.
- **Audience and prerequisites:** Who it serves and what they need to know.
- **Last reviewed:** YYYY-MM-DD
- **Limitations or access requirements:** Include when relevant.
```

Choose the applicable source type and format, and adapt the fields as needed. The review date should reflect when you actually checked the resource. Note paid access, required accounts, or version constraints when they affect its usefulness.

Link to the original source and write summaries in your own words. Attribute quotations and respect the source's license when including code or other material.

## Writing guides and examples

- Start with the purpose, intended audience, and prerequisites.
- Cite sources near the claims they support. Separate documented behavior, measured results, personal experience, and recommendations.
- State relevant dates and versions for time-sensitive claims. Explain the conditions and limits of reported results.
- For workflows, describe the inputs, steps, expected outputs, and how to verify success.
- For runnable examples, document setup, dependencies, required access, execution steps, and expected results. State what you tested and any parts you could not verify.
- Use lowercase, hyphenated file and directory names. Date time-sensitive surveys and comparisons with `YYYY-MM` or `YYYY-MM-DD`; leave stable explanations undated.

AI-assisted contributions follow the same standards. Check the claims, citations, and examples before submitting them.

## Submitting changes

1. Make a focused change on a branch. You can submit small additions and fixes directly as a pull request.
2. Check Markdown formatting, links, source attribution, and placement. Run any included example you changed when practical.
3. Update the relevant directory index so readers can find the contribution.
4. Open a pull request explaining what changed, why it belongs in the collection, and what you verified. Mention any remaining uncertainty.

Issues are also welcome for suggested resources, broken links, factual corrections, or larger organizational proposals. Include the relevant source or file and enough context for someone to act on the suggestion.

## Maintaining existing resources

Correct broken links and outdated claims with supporting sources. Update the review date when you recheck an entry. If a resource has become obsolete or unavailable, explain whether it should be replaced, removed, or retained for historical context.
