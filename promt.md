READ-ONLY. Diagnostics only. Do not change anything.

Two issues in CroProductionSummaryPDF.tsx:

ISSUE A - Confirm footer status: Show the CURRENT footer JSX (both 
occurrences) as it exists in the file right now. I need to verify whether 
the footer was changed to a centered "<title> • Page X of Y" with no logo, 
or if it still has the FHN logo + right-aligned page number.

ISSUE B - Month header overlap: In the "SUMMARY BY REVIEWER (MONTHLY COUNTS)" 
table, the month column headers (JAN-2026, FEB-2026, ... DEC-2026, TOTALS) 
are overlapping / colliding — the text is too wide for the columns. Show:
  1. The header row JSX for this monthly summary table — the month column 
     <Text> cells, how the month labels are generated/formatted (e.g. 
     "JAN-2026"), and their width styles (flexBasis / fixed width).
  2. The fontSize applied to these header cells.
  3. How many columns total (reviewer name + 12 months + totals = 14?).
  4. Whether the month label format is hardcoded or derived (so I could 
     shorten "JAN-2026" to "Jan" or "Jan '26" if needed).

Do not edit anything. Findings only.
