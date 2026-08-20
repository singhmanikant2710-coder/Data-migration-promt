READ-ONLY. Do NOT edit any file. Just find and report.

The PDFs (Initial Memo, Final Memo, CAS Linesheet) render "≥" as "e" and "≤"
as "d", but the review UI shows them correctly. This is a font glyph problem.

Do ONLY this and report back:

1. Search the whole frontend for every `Font.register(` call. For each, list:
   the file path, the fontFamily name, and the src (file path or URL).

2. Tell me the exact font used for the PDF body/narrative text in
   HtmlRichText.tsx, InitialMemoPDF.tsx, FinalMemoPDF.tsx, and the CAS Linesheet
   component. Is it Helvetica (a built-in) or an embedded .ttf file?

3. Is there a single shared font-registration file (like pageSetup.ts), or does
   each PDF component register its own font?

4. Does any registered .ttf actually contain glyphs for ≥ (U+2265), ≤ (U+2264),
   and en dash – (U+2013)? If the body font is Helvetica, say so — it lacks
   these glyphs and that is the bug.

Output just those 4 answers. No edits, no code changes.
