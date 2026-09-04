Bug 191 — PDF renders "≥" as "e" and "≤" as "d". READ-ONLY, no edits. One pass, answer, STOP.

When users paste ≥ (U+2265) or ≤ (U+2264) into rich-text fields, the generated PDF reports show them as "e" and "d" respectively. A DejaVu Sans Unicode font was previously registered to fix glyph rendering for ≥, ≤, and en-dash. This suggests some PDF component/field is NOT using that font, or the registration/mapping is missing for these fields.

Trace:
1. Find where the DejaVu Sans (or any Unicode font) is registered for @react-pdf/renderer. Search: Font.register, DejaVu, fontFamily. Which font family name is registered, and in which file(s)/setup?
2. Identify which PDF components render rich-text comment/description fields (the ones showing ≥/≤). Likely: HtmlRichText.tsx parser + ReviewPDF, FinalMemoPDF, InitialMemoPDF, CrmFindingsObservationsPDF. For the Comments/Description columns that show ≥/≤, what fontFamily is applied to those <Text> elements? Is it the registered DejaVu font, or a default (Helvetica) that lacks ≥/≤ glyphs?
3. Specifically: does the HtmlRichText parser / the rich-text render path set fontFamily to the Unicode font? Or does ≥/≤ fall through to Helvetica (which renders them as e/d because those glyphs are missing)?
4. Also check: is ≥/≤ possibly being character-substituted anywhere (a replace map, a hyphenation/normalization step) before render? Confirm whether it's a font-glyph issue or a character-substitution issue.

Report file paths + line numbers + the registered font family + which components/fields use vs don't use it. Do NOT fix yet.
