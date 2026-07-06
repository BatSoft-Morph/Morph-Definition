# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This repository holds the **Morph Protocol Definition** — the authoritative specification of the Morph Protocol, plus supporting presentations. It contains **no source code**: there is nothing to build, lint, or test. The `README.md` is one sentence; the `Licence.txt` is the substantive top-level document.

## Authoritative content lives in binary Office files

The specification itself lives in binary files:

- `Protocol/Morph Protocol.xlsx` — **the source of truth.** Structured bit-table tabs (`Connection`, `LinkType`, `LinkType Data`, `ValueType`, `SimpleType`, `LinkSequence`) that define the protocol. This is where active editing happens; when it and the docx disagree, the xlsx wins.
- `Protocol/Morph Protocol.docx` — prose overview of what Morph is. Its "How to read Morph Protocol.xlsx" section is **outdated** (wrong column order and tab list); use it for concepts, not for the current table layout.
- `Presentations/Introduction/A brief overview of the Morph protocol.pptx` — overview deck.
- `Presentations/Topology/Topology.vsd` (Visio source) + `Topology.pdf` (rendered).

**Reading these directly (no need to ask the user to paste):** the Office files are ZIP+XML. Unzip, then parse the XML — for `.xlsx`, resolve `xl/sharedStrings.xml` and lay each `xl/worksheets/sheetN.xml` back out as a grid (watch for empty self-closing `<c/>` cells when writing a cell regex). `.docx` text is the `<w:t>` runs in `word/document.xml`. `.pdf` is readable via the Read tool. Only `.vsd` (Visio) still needs the user.

**How to read the xlsx bit-tables:** each decode tab lists sections delimited by a type name in column A; read a section top-down, one row per line. A set bit turns its flag (column M) true; a `Name1/Name2` flag pair means *bit 0 → Name1, bit 1 → Name2*; a value (column ~P) is read only when its flag/condition (column B) holds. Bit columns are labelled 7…0 in the header row and shift per tab (`LinkType` C–J, `ValueType` D–K, `SimpleType` E–L; `SimpleType` also uses exponent-letter fields where e.g. `ValueSize = 2^z`). Verify a decode against each tab's built-in "Examples" block. Do not guess contents from filenames or commit messages — read the file.

## Licence constraints on edits

Per `Licence.txt`, **only Peter Thönell may alter or extend the Morph Protocol Definition.** In practice, for this repo that means:

- Never propose changes to the *substance* of the protocol on your own initiative. You may help draft, restructure, or review changes the user is already making.
- Editorial help (wording, consistency, formatting) is fine when the user asks.
- The `Presentations/` material illustrates the protocol; it must stay consistent with `Protocol/` — flag discrepancies rather than silently reconciling them.

## Working conventions in this repo

- **No `Specifications/` folder.** The global working agreement says design docs live under `Specifications/` "unless the project CLAUDE.md says otherwise" — here they live in `Protocol/` and `Presentations/`. Do not create a `Specifications/` folder; edit the existing documents instead (with the licence constraint above).
- **Binary files, no diffs.** Because the authoritative files are `.docx` / `.xlsx` / `.pptx` / `.vsd`, `git diff` shows nothing useful. Rely on the user's description of what changed, and on commit messages, for change history.
- **Branch naming reflects the current spec workstream.** For example, `Reworking-Data-link` — recent work has been reshaping value types, simple types, parameters, and data-link flags. When resuming a session, check the branch name and recent commits for the current focus.
