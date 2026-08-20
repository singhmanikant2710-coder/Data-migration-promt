READ-ONLY — DO NOT EDIT. Investigation only.

In the CASRR memo PDF (Customer Background / rich-text sections), after our
recent entity-decoding fix, stray "&" characters now appear in the rendered
output, e.g. "Parks Holdings, LLC.&", a lone "&" on its own line, and
"TTBA/Lending Authority: &". Also a stray "-" appears ("common ownership.-").

I need to know WHY, before deciding if this is a defect or correct rendering
of the source data. Report only, no edits.

1. Pull the raw stored value for this review's Customer Background field
   (read-only DB access on Dev). Show the exact raw string around
   "LLC." and "Lending Authority" — I need to see whether the source contains
   "&", "&amp;", "&amp;nbsp;", "&nbsp;", or a double-encoded entity there.

2. In HtmlRichText.decodeEntities(), trace what happens to these specific
   inputs, step by step through the multi-pass loop, and show the final output:
     a) "LLC.&nbsp;"
     b) "LLC.&amp;nbsp;"      (double-encoded)
     c) "&amp;&amp;"
     d) "authority: &amp; "
   For each, tell me the exact final string and how many passes it takes.

3. Confirm the ORDER of replacements in decodeEntities relative to the new
   named-entity block: specifically, is there any input where "&amp;" runs and
   produces a bare "&" that a later/earlier pass then fails to combine with a
   following "nbsp;" or "amp;" — i.e. a double-decode or half-decode path.

4. State clearly: is the stray "&" (and "-") most likely (A) faithful
   rendering of literal "&"/"-" that genuinely exists in the source data, or
   (B) an artifact of our decode changes. Give the evidence for your conclusion.

Output: raw source snippet + the 4 trace results + your A/B conclusion. No edits.
