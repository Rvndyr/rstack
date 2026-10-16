---
name: pr
description: Use when writing or opening a pull request, when asked for a PR description or PR body, or when the user types /pr
---

# PR

A reviewer already has the diff open. The PR body shows its shape, proves it works, and says how to undo it.

## Body structure

Start at the first heading. No preamble. Use the user's domain terms.

Use `##` headings. If the repo has `.github/pull_request_template.md`, keep its title and its sections (usually Purpose, Jira card, Testing), fill them, then add the sections below. The template's Testing section takes the Evidence content, so there is no separate Evidence heading. Summary, Changes and Merge Danger follow it. If the repo has no template, use Summary, Changes, Evidence and Merge Danger.

Link only URLs that appear in the conversation, the branch or the repo. For a Jira key with no known URL, write the bare key (`EAT-3747`).

## Summary

One small visual placed next to one or two sentences. Default to Mermaid, because GitHub renders it in PR bodies. Use `flowchart` for structure and `sequenceDiagram` for runtime flow. Keep it to about 15 nodes. Pick the smallest view that makes the point:

- pseudocode for logic
- a call tree or component tree
- a shallow file tree for a refactor
- a `diff`-shaped sketch of a tree or flow when the shape already exists
- Mermaid for interaction between components

Skip the visual when the change is a few lines in one file. Write one sentence instead.

## Changes

One entry per logical change, in this form:

````markdown
**`path/to/file`** One line naming the change.

```diff
# <plain-English note on what this change does>
-old
+new
```

One or two sentences: what the change does, and why it is needed.
````

Group repeated edits across files (for example the same tag in five files) into one entry that shows one example and lists the other files.

## Evidence

Before and after. Use a screenshot for visual changes. Otherwise show the exact test that failed and now passes, or the console output that changed. "Tests pass" alone is a claim, not evidence. If nothing was run, write "Not run" and name the command a reviewer should run.

## Merge Danger

- **Door:** one-way or two-way. A two-way door can be walked back. Destructive actions and hard-to-reverse decisions are one-way.
- **Rollback:** the exact steps (for example `git revert <sha>` then redeploy), and what a revert cannot undo (data already written in a new format, messages already sent, resources a deploy already replaced).
- **Blast radius:** what breaks if the change is wrong, and who notices.

The author is grading its own work. Support the door call with something observable: the diff, the template, the deploy behavior. Say what you could not verify.

## Finish

1. Show the draft body.
2. Ask before running `gh pr create --body-file`. Never push or open the PR without a yes.

## Mistakes

| Mistake | Fix |
|---|---|
| A paragraph describing the diff | A visual plus one sentence |
| Changes listed as before/after text with no code block | Use the code block entry above |
| "Low risk" with no reason | State the door, the rollback steps and the blast radius |
| Evidence is "tests pass" | Show the failing then passing test or the output |
| Diagram of a tiny change | Skip it |
