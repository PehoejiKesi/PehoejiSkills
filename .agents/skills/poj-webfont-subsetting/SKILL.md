---
name: poj-webfont-subsetting
description: Preserve POJ clusters when subsetting and serving webfonts. Use when a desktop font works but its WOFF2, CSS unicode-range slices, or a particular web weight loses letters or mark positioning.
---

# Preserve POJ in webfont subsets

Compare the full source font, the generated subset and the face actually rendered by the browser. Use the same strings and weight at each step to locate the first change.

## Build a subset that can shape a whole syllable

1. Collect the site's text and the repertoire its inputs accept. Include both NFC and NFD spellings, uppercase, syllabic nasals and ⁿ. A static page may contain ó͘ while a visitor types o + U+0301 + U+0358.
2. Keep bases, precomposed vowels and combining marks together in a POJ-capable subset. Include U+030D, U+0358 and U+207F, and the tone marks used by the target text, including breve or double acute where supported. Preserve the layout closure that pulls in unencoded glyphs such as contextual alternates.
3. Follow the project's existing build. With pyftsubset, keeping `--layout-features='*'` is a useful baseline. Compare the subset's cmap and shaped output with the full font, including dotless-i substitution, mark anchors and kerning. Glyph IDs can change during subsetting.
4. Inspect each shipped weight and its CSS descriptors. Match the actual cmap to the declared `unicode-range`. Overlapping Latin and regional slices can select different files for plain o and toned o͘. If the font also supports ideographic variation sequences, check format 14 mappings separately when that coverage is in scope.

## Verify the served result

Load the page with its real stylesheet and WOFF2 files. Wait for fonts to load, then inspect network requests and the browser's rendered-font information for a short test run. A successful download or the expected computed font-family only confirms part of the path.

Compare NFC and NFD for Ō͘, ŏ͘, i̍, m̄ and piⁿ in regular and other shipped weights. Capture connected text with the dotted vowel followed by a letter, such as khì-o͘h. If the standalone subset works but the page fails, isolate the same WOFF2 in a single-face test page and restore the production range and weight rules one at a time. Recheck the affected page and viewport after the fix.
