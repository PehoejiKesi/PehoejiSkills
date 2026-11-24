# PehoejiSkills

Twelve agent skills for POJ font engineering, Taigi language processing, and document publishing. The nine language and publishing skills come from [Taigibun Agent Skills](https://github.com/Taigibun/taigibun-agent-skills).

## Skills Included

### POJ font engineering

* [poj-mark-design](.agents/skills/poj-mark-design/SKILL.md) guides the design and attachment of tone marks, dotted o and superscript nasal forms.
* [poj-shaping-audit](.agents/skills/poj-shaping-audit/SKILL.md) checks normalization, positioning and fallback in built fonts. Its example commands use the [PehoejiFont tools](https://github.com/PehoejiFont/PehoejiFont.github.io/tree/main/tools).
* [poj-webfont-subsetting](.agents/skills/poj-webfont-subsetting/SKILL.md) tracks missing or misplaced marks through WOFF2 subsetting and browser font selection.

### Pipeline
*   **taigi-doc-pipeline**: The end-to-end orchestrator. Legacy Word `.doc` → fixed Unicode text → structured Markdown → GenYoMin2 TW `.docx` → outlined PDF, with the `NN_<title>_<descriptor>` naming convention, all five stage scripts, the stage-2 Kotlin fixer, and notes on what `textutil` does to old Word files.

### Language & Orthography
*   **taigi-roman-orthography-converter**: Conversion between POJ/KPL and Input/Unicode formats, tone placement rules, and the KonvertToPoj API.
*   **taigi-han-lo-formatter**: Han-lo spacing, hyphen and punctuation rules, plus how to protect English words, abbreviations and decimal points from the normalizer.
*   **taigi-legacy-encoding-converter**: Recovery of legacy Taigi encodings: number formats, font hacks (TP tws21.ttf, HOTSYS, CTS) and Big5-era kana stranded in the Private Use Area.

### Document & Typography
*   **taigi-document-structurer**: Heuristics and a script that turn flat Taigi text into Markdown with YAML title/author, part and chapter headings, and verse line blocks.
*   **taigi-document-typesetter**: The correct pandoc invocation for POJ text (smart punctuation off), a GenYoMin2 TW reference.docx builder, and per-run font/kinsoku patching.
*   **taigi-docx-typography-expert**: OpenXML line breaking for mixed-script text: native non-breaking hyphens, kinsoku, element-order traps.
*   **taigi-pdf-outliner**: docx → font-independent PDF through Microsoft Word (AppleScript) and Ghostscript outlining. Explains why LibreOffice must not be used for this font.

### Reference
*   **taigi-terminology-guide**: Standardized Taigi technical and linguistic terms.

## Installation

Clone the collection and open it as your workspace.

```sh
git clone https://github.com/PehoejiKesi/PehoejiSkills.git
cd PehoejiSkills
```

Codex and Antigravity discover the skills in `.agents/skills/`. Claude Code discovers them in `.claude/skills/`. For example, invoke `$poj-mark-design` in Codex or `/poj-mark-design` in Claude Code and Antigravity.

To use an individual skill in another project, copy its entire directory, following links so that scripts, references and assets travel with it. For example, from that project's root, with PehoejiSkills cloned alongside it:

```sh
mkdir -p .agents/skills .claude/skills
cp -RL ../PehoejiSkills/.agents/skills/taigi-doc-pipeline .agents/skills/
ln -s ../../.agents/skills/taigi-doc-pipeline .claude/skills/taigi-doc-pipeline
```

For Gemini CLI, install an individual skill from the local clone, for example:

```sh
gemini skills install ./taigi-roman-orthography-converter --scope user
```

## Workflow Example: Publishing a Taigi Book

`taigi-doc-pipeline` runs these in order; each stage has its own skill with the detailed rules:

1.  **Extract**: `textutil` turns the legacy `.doc` into UTF-8 text (`01_<title>_raw.txt`).
2.  **Fix**: `taigi-legacy-encoding-converter` recovers font-hack text and PUA kana; `taigi-han-lo-formatter` normalizes tone marks, Han-lo spacing and punctuation (`02_<title>_fixed.txt`).
3.  **Structure**: `taigi-document-structurer` inserts YAML title/author, headings and verse blocks (`03_<title>_structured.md`).
4.  **Typeset**: `taigi-document-typesetter` and `taigi-docx-typography-expert` produce a Word document with GenYoMin2 TW, non-breaking hyphens and kinsoku (`04_<title>_typeset.docx`).
5.  **Outline**: `taigi-pdf-outliner` exports through Microsoft Word and vectorizes every glyph with Ghostscript, so the PDF renders without the font installed (`05_<title>_outlined.pdf`).

## Prerequisites

*   **macOS** for stages 1 and 5: `textutil` (built in) and **Microsoft Word for Mac** driven by AppleScript (first run asks for Automation permission).
*   **pandoc** and **Ghostscript**: `brew install pandoc ghostscript`. **poppler** for rendering checks: `brew install poppler`.
*   **Python 3** (standard library only) for the structurer and typesetter scripts.
*   **JDK 17 or 21** for the stage-2 Kotlin fixer, with the `KonvertToPOJ` and `KonvertLegacyPOJ` repositories as git submodules (they are not on Maven Central).
*   **GenYoMin2 TW** fonts from https://github.com/ButTaiwan/genyo-font installed in `~/Library/Fonts/`.
*   **LibreOffice** only if you want `.odt` copies. Do not use it for PDF.

## Contributing

We welcome contributions! Please submit a pull request or open an issue to suggest improvements or add new skills.
