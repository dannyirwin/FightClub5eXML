<h1 align="center">Fight Club 5e XML</h1>

FightClub5eXML is a collection of XML source files for Dungeons & Dragons 5 and 5.5th Edition content that can be compiled into one a compendium — an importable XML file for use in apps such as Fight Club 5e, Game Master 5e, and Character Craft 5.5e.

<p align="center">
  <a href="https://github.com/vidalvanbergen/FightClub5eXML/releases/latest">
    <img src="https://img.shields.io/github/v/release/vidalvanbergen/FightClub5eXML?label=Latest%20Release&style=for-the-badge&color=FF3377" alt="Latest Version">
  </a>
  <a href="https://github.com/vidalvanbergen/FightClub5eXML/releases/nightly">
    <img src="https://img.shields.io/github/last-commit/vidalvanbergen/FightClub5eXML?style=for-the-badge&color=FF3377&label=Latest%20Nightly" alt="Latest Nightly">
  </a>
</p>

## How-to Use This Repository

The files listed in this repository as-is are not compatible with Fight Club 5e. They are instead a collection of individual source files that must be compiled together into a "compendium". That resulting compendium can then be imported into and used by Fight Club 5e.

This document makes a distinction between a **compendium** file and a **collection** file. A compendium file is what you ultimately import into Fight Club 5e; it is an XML file that contains all of the source data and is in a format that Fight Club 5e can process. A collection file is the raw source data that exists within this repository, and is not in a format that can be imported into Fight Club 5e. You must first compile a collection file into a compendium file.

This repository contains several collection files which can be found within the `Collections` folder. It's worth opening some of those files and noting the data found within them along with their basic structure. Each of those collection files contain entries that point to raw source data file found within the `Sources` folder.

### Download and Extract the Repository to Your Computer

Click on the green "Code" button towards the top of the page, and then click on the "Download ZIP" button on the subsequent modal popup. Extract the ZIP archive to your `Documents` folder. On Windows, this will be `C:\Users\YOUR_USER_NAME\Documents`; on macOS, this will be `/Users/YOUR_USER_NAME/Documents`.

The location on your computer that you extract this repository to will be referred to as the **repository root** folder. The path to the repository root should be something like `C:\Users\YOUR_USER_NAME\Documents\FightClub5eXML-master` or `/Users/YOUR_USER_NAME/Documents/FightClub5eXML-master`.

### Install `xsltproc`

You will need to install the `xsltproc` program in order to compile a collection into a compendium.

#### Windows

1. Install `chocolatey` by following the [official instructions](https://chocolatey.org/install).
1. Open up PowerShell with administrative privileges, and execute the following: `choco install xsltproc`.

#### macOS

1. Install `homebrew` by following the [official instructions](https://brew.sh/).
1. Open up Terminal and install `libxslt`: `brew install libxslt`.

#### Linux

You should be able to use your distro's official package manager to install either `xsltproc` or `libxslt` if `xsltproc` isn't available as a standalone package.

### Compile a Collection Into a Compendium

Open a command-line terminal (such as PowerShell on Windows or Terminal on macOS) and navigate to the repository root. You can do so by executing `cd C:\Users\YOUR_USER_NAME\Documents\FightClub5eXML-master` on Windows, or `cd /Users/YOUR_USER_NAME/Documents/FightClub5eXML-master` on macOS.

Next, execute the `xsltproc` program to compile a collection file into a compendium file. For example, if you wanted to compile the `WotC_5e_only.xml` collection, you would execute the following command:

```bash
xsltproc --xinclude -o Compendiums/WotC_only.xml Utilities/merge.xslt Collections/WotC_5e_only.xml
```

After that command has completed, you should see a file called `WotC_only.xml` in the newly created `Compendiums` folder. You can then import it into Fight Club 5e.

#### Helper Script and Batching

The `build-collections.sh` script compiles one or more collection files from `Collections/` into importable compendiums under `Compendiums/`.

```bash
Usage:

./build-collections.sh [-5.5e] [--validate] [-h/-?/--help] [collection_names...]
  -5.5e             Remove `[5.5e]` from the generated compendiums (optional; see note below).
  --validate       Validate each output file against `Utilities/compendium.xsd`.
  collection_names Optional list of specific collections to compile (filenames only, e.g. `WotC_5e_only.xml`).

If no collection names are provided, all XML files in the `Collections` directory are processed.

Examples:
  ./build-collections.sh
      Compile all collections.
  ./build-collections.sh -5.5e
      Compile all collections and strip `[5.5e]` from the output text.
  ./build-collections.sh --validate Dirwin_5.5e+Homebrew.xml
      Compile one collection and validate the result.
  ./build-collections.sh collection1.xml collection2.xml
      Compile only the listed collections.
```

**Note:** On Linux and macOS, collections whose names contain `_5.5e` but not `_5e` automatically get a second output file with `_[UNTAGGED]` in the filename, where `[5.5e]` suffixes in names and text are stripped. That gives you both a tagged and an untagged build without passing `-5.5e`.

For Windows, `WIN-build-collections.bat` uses `-2024` for the same “strip 5.5e tags” behavior as documented in that script’s help text.

## Custom Content

See the [Sources README](SOURCES.md) for element reference (spells, items, subclasses, and so on) and for building your own compendium from selected sources.

### Adding new content (short workflow)

1. **Author source XML** under `Sources/` using `<compendium version="5" ...>` in each content file. For subclasses, add optional `<feature>` blocks under a `<class>` whose `<name>` matches the base class in your collection (for example `Rogue [5.5e]` for the 2024 Player’s Handbook Rogue). See [SOURCES.md](SOURCES.md) for formats and merge behavior.
2. **Add a source manifest** (`source-<abbrev>.xml`) with a `<source>` root, metadata, and `<collection><doc href="..."/></collection>` listing your content files.
3. **Wire the pack into a collection**: either add an `<xi:include>` to a shared homebrew collection such as `Sources/DND_5.5e/Homebrew_5.5e/collection-homebrew_5.5e.xml`, or reference your `source-*.xml` from a custom file in `Collections/` (see [SOURCES.md](SOURCES.md), “Build Your Own Compendium”).
4. **Build** with `./build-collections.sh <YourCollection>.xml` and import the file produced under `Compendiums/`. Do not edit generated `Compendiums/` files by hand; regenerate after source changes.

Validate changes as you go: compendium fragments with `xmllint --noout --schema Utilities/compendium.xsd <file>`; collection files with `xmllint --noout --xinclude --schema Utilities/collection.xsd <collection-file>` (XInclude matches how the merge runs).

### Example: Rogue Subclass — Swashbuckler (2024 Conversion)

This repo includes a homebrew **Rogue Subclass: Swashbuckler (2024 Conversion)** intended for the 2024 Rogue (`<name>Rogue [5.5e]</name>` in the merged class). It lives under `Sources/DND_5.5e/Homebrew_5.5e/Misc/Swashbuckler_2024/` (`source-swash24.xml`, `class-rogue-swash24.xml`, `items-swash24.xml`). It is wired into `Sources/DND_5.5e/Homebrew_5.5e/collection-homebrew_5.5e.xml` and included in the sample collection `Collections/Dirwin_5.5e+Homebrew.xml`.

**How to use it in the app:** Build a compendium that includes the 2024 Player’s Handbook Rogue **and** this pack (for example `./build-collections.sh Dirwin_5.5e+Homebrew.xml`), then import `Compendiums/Dirwin_5.5e+Homebrew.xml` into Fight Club 5e / Game Master 5e / Character Craft 5.5e. When creating a Rogue, choose the optional subclass labeled **Swashbuckler (HB)** (HB distinguishes this conversion from legacy “Swashbuckler” entries in other sources). The pack also adds an optional homebrew item **Davix Rapier [5.5e]** if you want the sample rapier.

## Contributing

If you'd like to contribute, feel free to fork the repository and submit pull requests with your additions and changes.

## Additional Contributors

`@kinkofer` for XML generation systems to allow github collections to be auto generated.

`@felix_mil_` for XML creation tools [https://felixmil.shinyapps.io/compendiumbuildr/](https://felixmil.shinyapps.io/compendiumbuildr/).

`@rrgeorge` and `zamrod` for their JSON to XML scripts.

`@MrFarland` for Artificer Infusions and other XML.

`@fightclub5exml` and `@dragonahcas` for carrying the mantle.

`@zcdziura` for answering user's questions.

`@sheppe` for the Widows bat file.

`@the_archivist` for adding various sources to the compendium.  
(You can find their collection of compendiums on [patreon.com/archivist5](https://patreon.com/archivist5))

`@vidalvanbergen` for adding various sources and maintaining the repository.

`@recco` for adding various homebrew sources to the compendium.

`@nikjft` for converting legacy content to the 2024 format and adding to the utilities.

[`@Iggwilv`](https://archive.org/details/the-wild-beyond-the-witchlight) for adding several adventures.

[`@DM-Velek`](https://www.reddit.com/user/DM-Velek/) for adding content and adventures.
