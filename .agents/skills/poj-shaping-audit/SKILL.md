---
name: poj-shaping-audit
description: Diagnose POJ text rendering in built fonts using Unicode normalization, HarfBuzz geometry, visual specimens and target-engine fallback checks. Use when evaluating coverage or investigating missing, floating, doubled or colliding marks.
---

# Audit POJ shaping

Record the exact font file, face, weight or axis settings and failing string. Preserve its original code points. Compare NFC and NFD from the same input. For toned dotted o, also try the typed sequence with U+0358 before the tone. Avoid NFKC for these samples because it folds ⁿ into an ordinary n.

## Reproduce on the built face

From the PehoejiFont repository root, set `font` to the built TTF and run the following with Python, fontTools, uharfbuzz and Pillow available.

```sh
out=$(mktemp -d)
python tools/poj_font_audit.py "$font" > "$out/coverage.json"
python tools/poj_shape_test.py "$font" > "$out/shape.json"
python tools/poj_specimen.py "$out" "$font"
```

Open the generated PNG. In another repository, use its equivalent shaping and specimen tools. Exercise a e i o u o͘ and their capitals, with tones. Include Ó͘ Ō͘ A̍ I̍ M̍ N̍g, m̍ n̍ ḿ m̄ m̂ n̂ ǹ n̄, ⁿ and i̋. Compare paired NFC/NFD rows and read the following line at normal size for overhang, punctuation and double-hyphen spacing.

> Chhù-piⁿ tńg--khì-o͘h, saⁿ-koan lóng hó-sè, kiⁿ-á-ji̍t chin hoaⁿ-hí!

## Locate the failure

- **Missing ink or unexpected style.** Check cmap coverage and glyph zero in the shaped output. In a browser or native app, inspect which face drew each run. A fallback font can disguise a missing character.
- **Wrong placement.** Inspect pen positions, offsets, advances and ink bounds for the failing cluster and its neighbor. Check the actual base and mark in the applicable lookup. A feature tag alone says little about coverage. Zero offsets can be intentional and bounding-box overlaps need visual confirmation.
- **Different normalization results.** Compare the visible outlines and total advance. Different glyph counts can produce equivalent output. Check whether a partially composed base such as Ŏ still accepts the right-hand dot.
- **Engine-specific failure.** Replay the same strings and face in the affected renderer. On macOS the repository's tools/poj_coretext_compare.py compares CoreText runs with HarfBuzz and records fallback fonts. Report which engines and instances were exercised.

After a repair, rerun the original failing string and the full specimen against the rebuilt artifact. Record the string, input form, face and visual evidence with the result.
