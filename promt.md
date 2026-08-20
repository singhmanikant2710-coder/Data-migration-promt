READ-ONLY — DO NOT EDIT. Investigation only. Do not query the DB.

I now HAVE the raw source. The rendered memo shows stray "&" characters that
DO NOT exist in the source. So this IS a rendering defect introduced by our
decode/parse chain, not faithful source rendering. Your earlier "literal &"
conclusion was wrong — confirmed against raw data.

Raw source (relevant part) is clean, e.g.:
  "...and Parks Holdings, LLC. Parks Holdings, LLC is a partnership that was
  formed in Dec. 2009..."
  "TTBA/Lending Authority: The current TTBA of $129,313M..."

But the memo renders:
  "...Parks Holdings, LLC.&"   then a lone "&" line   then next paragraph
  "TTBA/Lending Authority: &"  then next paragraph
  "...common ownership.-"

The actual stored HTML for Borrower_information almost certainly contains
double-encoded entities (e.g. "&amp;nbsp;") and/or empty paragraphs used as
spacers. I need you to trace, NOT guess:

1. In HtmlRichText.decodeEntities(), run these inputs through the ACTUAL
   multi-pass loop as currently written (post our changes) and show the exact
   final output plus pass count:
     a) "LLC.&amp;nbsp;"
     b) "&amp;nbsp;"
     c) "<p>&amp;nbsp;</p>"
     d) "ownership.&amp;ndash;"   (or &amp;#8211;)
   For each, show what each pass produces line by line. I expect to see a bare
   "&" surviving — confirm exactly where and why (order of &amp;-last vs the
   &nbsp;/named-entity replacements, and the fact that a produced "&" is never
   re-combined with a following "nbsp;").

2. Confirm whether the pre-decode block we added in parseHtmlToAst (CHANGE 2)
   interacts with this — i.e. does decodeForTags leaving &amp; alone, followed
   by decodeEntities, produce the stray "&"? Show the combined path for
   "<div>LLC.&amp;nbsp;</div>".

3. Identify the MINIMAL fix location. I want the smallest change that makes a
   produced "&" correctly re-participate in entity decoding across passes
   (so "&amp;nbsp;" fully collapses to a space instead of leaving "&"), WITHOUT
   breaking the "&amp; must be last" invariant and WITHOUT over-decoding a
   genuinely-literal "&" that has no entity suffix (e.g. "AT&T" must stay "AT&T",
   "C&I" must stay "C&I").
   NOTE: the source literally contains "C&I of $84.6MM" and "Stuart Parks, &
   Chester Michael" — these ampersands are REAL and must be preserved. Only
   ampersands that are the decoded head of a double-encoded entity
   (&amp;nbsp;, &amp;amp;, &amp;ndash; etc.) should collapse.

4. Also explain the stray "-" after "ownership." — is it a decoded
   "&amp;ndash;"/"&amp;#8211;" losing its tail, or an empty list-item bullet?
   Point to the exact code path.

Output: the 4 trace results + the single minimal fix location + how it
distinguishes real "&" (C&I, AT&T, "Parks, & Chester") from double-encoded
entity heads. No edits yet.
