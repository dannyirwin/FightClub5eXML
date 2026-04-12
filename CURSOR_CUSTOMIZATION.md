# Cursor Customization Starter (for experienced web developers)

This repository did not previously include project-level Cursor rules.  
The files under `.cursor/rules/` now provide a practical baseline you can tune.

## What was added

- `.cursor/rules/00-project-context.mdc`
  - Always-on repo context so the agent knows the XML/collection build model.
- `.cursor/rules/10-xml-editing-guardrails.mdc`
  - XML-specific safety guidance, scoped to `Sources/**` and `Collections/**`.
- `.cursor/rules/20-agentic-workflow-defaults.mdc`
  - Always-on workflow expectations for focused implementation + verification.

## How to tune these quickly

### 1) Adjust strictness

- For stricter behavior, set `alwaysApply: true` on specialized rules.
- For less noise, keep specialized rules scoped through `globs`.

### 2) Make the assistant feel more "senior"

In `20-agentic-workflow-defaults.mdc`, add lines such as:

- "Prioritize impact/risk analysis before changing shared data structures."
- "When there are multiple valid approaches, present the default and one alternative."

### 3) Optimize for your preferred loop

If you like rapid micro-iterations, add:

- "Run a minimal check after each edit batch, then continue."

If you like fuller checkpoints, add:

- "Batch edits by subsystem, then run one comprehensive validation pass."

### 4) Add personal prompt templates

Create files in `.cursor/prompts/` for recurring tasks. Example prompt names:

- `review-xml-change.md`
- `build-failure-triage.md`
- `safe-refactor-plan.md`

Use each template as a starting point instead of rewriting prompts from scratch.

## Suggested next customizations

- Add a rule scoped to `Utilities/**/*.xslt` for XSLT refactors.
- Add a rule for `README.md`/docs changes that enforces concise changelog entries.
- Add task-specific prompts for:
  - "Add a new source book cleanly"
  - "Migrate legacy source to 2024 format"
  - "Debug merge/build failures from include paths"
