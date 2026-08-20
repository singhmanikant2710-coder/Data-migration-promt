READ-ONLY DIAGNOSTIC — DO NOT EDIT ANY FILE. Investigation only.

Context: In the CASRR review-info memo PDF, Rich Text fields that contain
pasted content from Word/Excel are rendering incorrectly. Specifically:
  (a) HTML tags/entities appear as literal text in the PDF — e.g. "<div>",
      "<span style=\"font-size: 0.875rem\">", "&nbsp;" show up verbatim.
  (b) "≥" and "≤" symbols render as "e" and "d".
  (c) Pasted tables stretch to full page width.
  (d) Pasted images enlarge / lose their original size.

I need to understand the current Rich Text → PDF rendering pipeline before
proposing any fix. Report back ONLY findings, no edits.

Please investigate and report:

1. Which component(s) render Rich Text / HTML field values into the
   @react-pdf/renderer memo PDF? Search under
   src/app/review/[ecif]/review-info/components/ for any code that takes an
   HTML string and outputs PDF nodes (look in CrmFindingsAndRatingsSection,
   RiskRatingJustificationSection, ReviewInfoSection, and any shared
   html/richtext helper).

2. Is there an existing HTML-parsing / html-to-react-pdf utility in the
   codebase (e.g. a parser, sanitizer, or a "renderHtml"/"parseRichText"
   helper)? List the file path(s) and show how it maps HTML tags → PDF
   <Text>/<View> nodes. If tags like <div>/<span> are NOT in its tag map,
   note that.

3. How are HTML entities decoded (&nbsp;, &ge;, &le;, &amp;)? Find any
   decode/replace logic and show the exact mapping table. Confirm whether
   &ge;/&le; (≥/≤) are handled at all.

4. For pasted tables: show how <table>/<tr>/<td> (or the field's table data)
   are converted to PDF, and where column/table width is set. Is width
   hard-coded to 100% / full page?

5. For pasted images: show how <img> or image data in Rich Text is rendered
   in PDF, and where width/height is derived.

6. Confirm which shared style object(s) these use, so I know what is shared
   vs. component-specific before any change.

Output format: for each of the 6 points, give the file path, the relevant
code snippet (read-only), and a one-line summary of what it does. End with a
short list of the root-cause file(s) for each of the 4 bugs (#191 entities,
#192 tags, #193 table width, #194 image size). DO NOT modify anything.
