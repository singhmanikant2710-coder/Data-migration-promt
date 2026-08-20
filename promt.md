READ-ONLY — DO NOT EDIT. Investigation only.

In Initial, Final AND Linesheet memo PDFs, the literal characters "≥" and "≤"
render as "e" and "d". IMPORTANT: the source input contains the ACTUAL
Unicode characters "≥" (U+2265) and "≤" (U+2264) as plain text — NOT the HTML
entities "&ge;"/"&le;". So our decodeEntities named-entity fix does not apply
to this input at all. The mangling to "e"/"d" happens somewhere else.

Trace, do not guess. Report only:

1. In HtmlRichText.tsx, follow a plain text run containing the literal
   character "≥" (U+2265) through parseHtmlToAst → parseText → decodeEntities →
   final render. Does any replace()/regex/slice touch or corrupt this char?
   Pay special attention to:
   - the numeric-entity regexes: /&#(\d+);/ and /&#x([0-9a-f]+);/gi — could a
     literal "≥" ever be misread here? (it shouldn't, but confirm)
   - any String.fromCharCode / charCodeAt / substring logic that could truncate
     a multibyte char to a single wrong letter.
   Show me the exact line, if any, that turns "≥" into "e".

2. Check the react-pdf FONT registered for these memo bodies. Which font family
   is used for the narrative text, and does that font actually contain glyphs
   for U+2265 (≥) and U+2264 (≤)? If the font lacks these glyphs, react-pdf may
   substitute or drop them — describe what happens. (A missing-glyph fallback is
   a strong candidate for ≥→e, ≤→d.)

3. Is there any pre-processing step (in the memo components, the data layer, or
   an API mapping) that runs a transliteration / ASCII-fold / "smart-char
   replace" over narrative text before it reaches HtmlRichText? Search for
   replace calls mapping ≥/≤ or \u2265/\u2264, and any "normalize"/"sanitize"
   util applied to these fields.

4. State your conclusion: is "≥"→"e" caused by (A) a font missing the glyph,
   (B) a string-mangling/slice bug on multibyte chars, or (C) an upstream
   transliteration step? Give the exact file+line evidence.

Output: the trace + font check + any transliteration step + your A/B/C
conclusion with evidence. No edits.
