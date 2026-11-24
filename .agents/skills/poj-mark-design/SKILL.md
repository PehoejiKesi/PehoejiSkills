---
name: poj-mark-design
description: Add or repair Pe̍h-ōe-jī glyphs and OpenType mark attachment in Latin font sources. Use for dotted o, tone marks, soft-dotted i, or superscript nasal design across typefaces.
---

# Design POJ marks in the source font

Start with the family's editable sources and normal build. Trace one working accented vowel from outline and anchors through the compiled font. Reuse its component, anchor naming and feature-generation conventions across the affected masters.

## Draw from the family's own forms

- **Dot above right (U+0358).** Start with the family's accent dot or i dot. Compare size and weight with its accents at reading size. Give the dot its own upper-right attachment on o and O so a centered tone can coexist with it. Test Ó͘ and Ō͘ against a following letter. Raising the dot above the entire accented outline can leave it floating.
- **Vertical line above (U+030D).** Draw an upright stroke with parallel sides and terminals that match the family. Use the family's acute, grave and circumflex as references for stroke length and the top edge of the finished cluster. Aim for the same accent height band on lowercase and uppercase bases. On dotless i, keep enough space above the stem to distinguish the stroke from a tittle at reading size. Center it optically over both narrow i and wide m.
- **Superscript n (U+207F).** Match the family's superior letters in height, stroke weight and sidebearings. Give ⁿ its own advance. Read piⁿ and saⁿ-koan to check spacing on both sides.

## Wire the marks into shaping

In an anchor-based design, classify combining marks in GDEF and give them zero advance. Use GPOS mark-to-base for independent top and upper-right positions. Use mark-to-mark where one mark actually attaches to another. Inspect the bases that reach positioning after substitution, including accented o/O, dotless i and any composed glyph that can still receive a tone.

For i̍ and i̋, use the family's soft-dotted substitution so only the tone remains above the stem. Keep required composition in `ccmp` when adding substitution rules. Test with optional ligatures disabled. If a composed dotted vowel replaces o, check its advance and kerning classes beside the next letter. Preserve any intentional spacing-dot design when repairing an existing family.

Build the affected faces. Compare NFC and NFD specimens with the original family's accents at reading and display sizes. Check the widest and narrowest bases, capitals and connected text. For variable sources, inspect relevant axis extremes and an intermediate instance before settling on anchor positions.
