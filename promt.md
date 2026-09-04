Bug 191 — professional fix prep. READ-ONLY, no edits. Answer, STOP. Goal: render ≥/≤ as actual symbols WITHOUT changing the document's overall font/spacing (a previous attempt that set DejaVu on the base Page style broke spacing/layout).

1. Confirm the @react-pdf/renderer version (package.json).
2. Does this @react-pdf version support Font.register with a `fonts` fallback array, or Font.registerHyphenationCallback-style fallback? Specifically: can we register a Unicode font as a FALLBACK that is used ONLY for glyphs missing in the primary Helvetica, so normal text stays Helvetica and only ≥/≤ use the fallback? Check @react-pdf docs/types in node_modules for a fallback/font-fallback feature.
3. In HtmlRichText.tsx (the rich-text parser that outputs <Text> runs), at what point are text runs emitted? Could we, at the parser level, detect ≥ (U+2265) / ≤ (U+2264) in a text run and wrap ONLY those characters in an inline <Text style={{fontFamily:'DejaVuSans'}}> while leaving surrounding text in the default font? Report where text runs are split/emitted so this is feasible.
4. Where are the base Page styles that currently rely on default Helvetica (so we can confirm we will NOT touch them)?

Report the version, whether native font-fallback exists, and the feasibility of character-level font wrapping in HtmlRichText. Do NOT change anything.
