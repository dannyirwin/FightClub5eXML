# Safe XML Edit Prompt

You are editing FightClub5eXML source files.

## Objective

Apply the requested XML change with minimal structural risk.

## Workflow

1. Identify the smallest set of affected XML files.
2. Mirror existing patterns from nearby entries.
3. Keep IDs/source refs stable unless explicit rename is requested.
4. Run a targeted validation/build command for the edited collection.
5. Return:
   - files changed
   - why the edit is safe
   - exact validation command and result

## Constraints

- Avoid broad formatting rewrites.
- Do not change unrelated entries.
- Preserve compatibility with collection include paths.
