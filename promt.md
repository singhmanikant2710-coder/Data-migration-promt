READ-ONLY. Diagnostics only. Do not change anything.

Two issues in the Initial Memo report (InitialMemoPDF.tsx), which also apply 
to Final Memo (FinalMemoPDF.tsx):

ISSUE 1 — Approver field width (cosmetic):
In the header/customer info area, the "Approver:" field value 
("HOULDITCH, GEOFFREY") wraps onto two lines. Geoff wants the field width 
increased so a long approver name fits on ONE line. Show:
- The header block JSX where "Status", "Reviewer", "Approver" fields are 
  rendered (the right column of the header).
- The width styles (flexBasis/width) of the Approver field and its 
  label/value container.
- How the three columns (Customer#/ReviewID/Sample | Sample Date/Completed/
  Distributed | Status/Reviewer/Approver) are laid out and their widths.

ISSUE 2 — Ampersand showing as "&amp;" (formatting):
In a long-text field, text shows the literal HTML entity "&amp;" instead of 
"&" (e.g. "Excluding net income &amp; distributions"). Also scorecard IDs 
show stray "&" fragments like "df6debd1-&a53d-&4d79". Show:
- The code/component that renders these long-text narrative fields (the CRO 
  concurrence bullet text).
- Whether the text is rendered as-is from data, or passed through any 
  HTML-decode / sanitize / rich-text helper.
- Where the scorecard ID is rendered and whether it's also passing through 
  the same rendering path (which might be inserting "&" fragments).
- Any existing helper in the codebase that decodes HTML entities 
  (e.g. &amp; -> &, &lt; -> <) that could be applied.

For BOTH issues, confirm whether InitialMemoPDF.tsx and FinalMemoPDF.tsx 
share code or are duplicated (so I know if both need editing).

Do not edit anything. Findings only.
