# Add homebrew source

Use this skill when a user wants to add new homebrew content to this repository.

## Goal

Create a new homebrew source pack in `Sources/`, wire it into the correct homebrew collection, and validate the XML.

## Ask for these inputs first

1. Ruleset: `DND_5e` or `DND_5.5e`
2. Source folder name (import-safe, no spaces/quotes in technical path)
3. Human-readable source metadata:
   - `name`
   - `abbreviation`
   - `publisher`
   - `description`
4. Content types to scaffold (`backgrounds`, `feats`, `items`, `spells`, `races`, `classes`, `bestiary`)

## Steps

1. Create a new source directory under:
   - `Sources/DND_5e/Homebrew/<Your_Source>/` or
   - `Sources/DND_5.5e/Homebrew_5.5e/<Your_Source>/`
2. Add `source-<abbr>.xml` with:
   - `<source>` root
   - metadata fields
   - `<collection>` with one `<doc href="..."/>` per content file
3. Add requested content files with `<compendium version="5" auto_indent="NO">`.
4. Add an include entry to the relevant homebrew collection file:
   - `Sources/DND_5e/Homebrew/collection-homebrew.xml` or
   - `Sources/DND_5.5e/Homebrew_5.5e/collection-homebrew_5.5e.xml`
5. Use:
   - `xpointer="xpointer(/source/collection/doc)"`
   - relative `href` path from the collection file location.
6. Validate changed XML:
   - `xmllint --noout --schema Utilities/compendium.xsd <content-file>`
   - `xmllint --noout --xinclude --schema Utilities/collection.xsd <collection-file>`
7. If validation fails, fix errors and re-run validation.

## Output format

Return:

- files created
- files modified
- exact validation commands run
- next build command (for example with `xsltproc` or `./build-collections.sh`)

## Constraints

- Do not edit `Compendiums/` outputs directly.
- Keep edits minimal and localized to the target source pack and collection wiring.
