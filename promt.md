READ-ONLY — DO NOT EDIT. Investigation only.

CONFIRMED root cause: In the review UI (edit view) the characters "≥" (U+2265)
and "≤" (U+2264) display CORRECTLY. But in the generated PDFs — Initial Memo,
Final Memo, AND CAS Linesheet — they render as "e" and "d". All three use
@react-pdf/renderer. This is a FONT GLYPH issue: the font registered for PDF
body text lacks glyphs for ≥/≤ (and likely en dash "–" and causes the
viewer-dependent "&" artifact too), so react-pdf substitutes wrong glyphs.

Report only, no edits:

1. List every Font.register(...) call across the PDF layer: HtmlRichText.tsx,
   InitialMemoPDF.tsx, FinalMemoPDF.tsx, the CAS Linesheet component, and
   pageSetup.ts (or wherever fonts are centrally registered). For each, show
   the font family name, the src (file path or URL), and which weights.

2. Identify the exact font file(s) currently used for narrative/body text.
   Are they standard PDF base fonts (Helvetica/Times) or embedded .ttf files?
   Confirm whether these files contain glyphs for U+2265 (≥), U+2264 (≤),
   U+2013 (– en dash). If they're the built-in Helvetica, note that its glyph
   coverage is limited and these chars will fail.

3. Is there ONE central place where fonts are registered and shared, or does
   each PDF component register its own? I want to know if a single font swap
   fixes all three, or if I must change each component.

4. Recommend the minimal fix: a single Unicode-complete embeddable .ttf
   (e.g. Noto Sans, DejaVu Sans, or Liberation Sans) that covers ≥ ≤ – and
   common symbols. State:
   - whether such a font file already exists in the repo (search assets/fonts,
     public/fonts, etc.)
   - the exact Font.register call(s) needed and which fontFamily declarations
     must point to it
   - whether this is a shared change (one location) or must be repeated per
     component
   Do NOT recommend changing decodeEntities — the input is a literal char, not
   an entity, so decoding is irrelevant here.

Output: font registration inventory + glyph-coverage gap + central-vs-per-
component + the single recommended font and exact wiring points. No edits.
