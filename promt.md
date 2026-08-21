Edit ONLY InitialMemoPDF.tsx, FinalMemoPDF.tsx, ReviewPDF.tsx. Auto-approve OFF.
Output diffs. Do NOT change pageSetup.ts (shared — would affect 15+ other PDFs).

DejaVuSans is wider than Helvetica, causing overlap in TWO specific places:
(a) the header bar (header VALUE at 11pt makes "Completed Date" value touch
"Reviewer"), and (b) the Account Information table (cells at 10pt clip "Bank" and
long account numbers). Reduce ONLY these to 9pt. Leave narrative, labels, section
tables, and footers unchanged.

InitialMemoPDF.tsx:
- styles.headerValue.fontSize: 11 -> 9
- styles.headerLabel.fontSize: 10 -> 9   (keep label/value same size in header)
- styles.thAcct.fontSize: 10 -> 9
- styles.tdAcct.fontSize: 10 -> 9
- Do NOT change styles.th/td (other tables), narrative (11), bodyLabel/bodyValue,
  HtmlRichText baseFontSize, or footnote.

FinalMemoPDF.tsx:
- Same four changes: headerValue 11->9, headerLabel 10->9, thAcct 10->9,
  tdAcct 10->9. Nothing else.

ReviewPDF.tsx (Account Information table is the wide bespoke one):
- Header card field Text elements currently { fontSize: 10 }: change to 9 (the
  Customer #, Review ID, Sample, dates, Status, Reviewer, Approver rows).
- In the Account Information table ONLY: the tiny/narrow header and value cells
  that use { fontSize: 10 } inline -> 9.
- Do NOT change: styles.page (already 9), narrativeText (9), the section tables
  (Ratings/Regulatory/Covenants at 11), valueInline/labelInline (11), or
  footerText (8).

Keep exactly one <Text> per cell, no duplicates. Show all three diffs.
