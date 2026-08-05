Single-file edit: frontend/src/components/pdf/HtmlRichText.tsx

BUG: The parser calls decodeEntities BEFORE tokenizing (in parseHtmlToAst: 
"html = decodeEntities(html)" at the top). This decodes "&lt;" to "<" before 
parsing, so the tokenizer treats that "<" as a tag start and swallows it plus 
following text until the next ">". Result: genuine "<" symbols (e.g. 
"< 5% growth", "DSC &lt;= 1.10x") and the text after them disappear.

FIX: Remove the top-level pre-decode so "<" is NOT decoded before parsing. 
Text-level decoding (in parseText) already handles entities within text nodes 
correctly, so genuine "&lt;" in text content will be decoded to "<" AFTER 
tokenization — at which point it's safely treated as literal text, not a tag.

In parseHtmlToAst, REMOVE this line (the top-level pre-decode):
    html = decodeEntities(html);

Keep the decodeEntities calls in parseText (raw text decoding) and parseAttrs 
(attribute decoding) — those are correct and needed.

CONSTRAINTS:
- Only remove the ONE top-level pre-decode line in parseHtmlToAst.
- Do NOT change parseText's decodeEntities call (that's what will now 
  correctly decode &lt; -> < in text content, after tokenization).
- Do NOT change parseAttrs, the tokenizer, or decodeEntities itself.
- Note the trade-off (acceptable): if any data contains ENCODED html tags 
  like "&lt;i&gt;", they will now show as literal text "<i>" rather than 
  being rendered as real italic. This is correct behavior — real formatting 
  should come from actual tags, and Geoff's issue is genuine "<" symbols in 
  text, not encoded markup.
- Only edit this one file. Show the change.




Single-file edit: frontend/src/components/pdf/ReviewPDF.tsx (CAS Linesheet)

BUG: Policy Exception Comments ([policy_exception_information]) don't show 
paragraph breaks. It's rendered as plain <Text>{stripHtml(...)}</Text>, and 
stripHtml removes <br>/<p> tags and collapses whitespace, losing paragraph 
structure. All other narrative fields route through HtmlRichText (which 
preserves paragraphs) and display correctly.

FIX: Route Policy Exception Comments through HtmlRichText, exactly like the 
other narrative fields (Risk Rating Justification, Scorecard Comments, etc.), 
so paragraph breaks (<p>/<br>) render properly.

Current render (plain Text + stripHtml):
    <Text style={[styles.longText, { fontSize: 11, lineHeight: 1.2 }]}>
      {policyExceptionInformation || "-"}
    </Text>

Change to use HtmlRichText with the RAW value (not the stripHtml'd version), 
matching the pattern used by the other narrative sections in this file:
    <HtmlRichText html={`<div style="font-size: 11px">${<raw policy exception source> || ""}</div>`} />

Use the SAME raw source that policyExceptionInformation was built from 
(before stripHtml was applied) — i.e. the original policy_exception_information 
HTML string. If the value is empty, keep a "-" fallback (either render "-" 
via a small conditional, or pass an empty div — match how other fields handle 
empty).

CONSTRAINTS:
- Route Policy Exception through HtmlRichText using the RAW (un-stripped) 
  source, mirroring the other narrative fields' exact pattern in this file.
- Keep the "-" fallback behavior for empty values.
- Do NOT change stripHtml itself (other places may still use it).
- Do NOT change other fields or layout.
- This fix depends on the Issue 1 HtmlRichText fix (so "<" renders correctly 
  in HtmlRichText now) — apply Issue 1 first.
- Only edit this one file. Show the changed render and confirm it uses the 
  raw source through HtmlRichText.
