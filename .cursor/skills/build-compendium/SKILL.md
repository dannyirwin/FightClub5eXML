# Build compendium

Use this skill when a user asks to compile one or more collection XML files into importable compendium XML output.

## Goal

Build selected collections into `Compendiums/`, optionally validate output, and clearly report artifacts and blockers.

## Inputs

1. Collection names (optional; if omitted, build all collections)
2. Whether to strip `[5.5e]` tags (`-5.5e`)
3. Whether to validate output (`--validate`)

## Steps

1. Confirm required tools:
   - `xsltproc`
   - `xmllint` (if validating)
2. If `xsltproc` is unavailable, stop and report the dependency issue with exact install guidance.
3. Build using the repository script when possible:
   - `./build-collections.sh [--validate] [-5.5e] [collection_names...]`
4. For a single direct build command, use:
   - `xsltproc --xinclude -o Compendiums/<output>.xml Utilities/merge.xslt Collections/<collection>.xml`
5. Confirm generated files in `Compendiums/`.
6. If validation was requested, run and report:
   - `xmllint --noout --schema Utilities/compendium.xsd Compendiums/<output>.xml`

## Output format

Return:

- commands run
- generated files
- validation results
- any blockers (missing dependencies, invalid XML, missing includes)

## Notes

- The script flag for stripping tags is `-5.5e`.
- Avoid outdated references to `-2024`.
