READ-ONLY. Diagnostics only. Do not change anything.

Three reports share overlapping formatting/layout issues (items #173 Initial 
Memo, #174 Final Memo, #175 CAS Linesheet):
- frontend/src/components/pdf/InitialMemoPDF.tsx
- frontend/src/components/pdf/FinalMemoPDF.tsx
- frontend/src/components/pdf/ReviewPDF.tsx  (CAS Linesheet)

Investigate all shared + specific issues (no edits):

=== SHARED ISSUE A — Scorecard ID wrap (all 3 reports) ===
The Scorecard ID has extra space characters injected after hyphens (a hack 
added to force text wrapping). It appears in TWO tables in each report: the 
"Scorecard Assessment" table and the "Account Information" table. Show:
1. How the Scorecard ID value is built/rendered in each report — is there code 
   injecting spaces after hyphens (e.g. .replace(/-/g, "- ") or similar)? Show 
   the exact transformation.
2. The Scorecard ID cell in the Scorecard Assessment table AND in the Account 
   Information table — the cell width (flexBasis) and text styling (wordBreak?).
3. Whether removing the injected spaces + using proper wrapping (wordBreak: 
   "break-all" or a width) would let the ID wrap naturally within the cell.
Show this for all three files (InitialMemo, FinalMemo, ReviewPDF).

=== ISSUE B — font size 11 for blue values (Memos #173/#174 only) ===
Show where "blue highlighted" values are rendered in InitialMemoPDF and 
FinalMemoPDF and their current fontSize (client wants 11).

=== ISSUE C — Account Information table starts on next page (Memos #173/#174) ===
For large tables, the Account Information table begins on the next page. Show 
the Account Information table's wrap/break/minPresenceAhead in both memos.

=== ISSUE D — Account Info header repeats mid-page (Memos #173/#174) ===
The Account Information table header repeats WITHIN a page (not just top of new 
pages), on pages 3-11. Show:
1. How the Account Info header is rendered in the memos — does it use "fixed" 
   prop? How are rows chunked?
2. Compare with CAS Linesheet (ReviewPDF.tsx) Detail/Account table header — it 
   does NOT have this problem. Show how ReviewPDF renders its table header 
   differently (this is the working pattern to copy).

=== ISSUE E — Transaction Information empty space (CAS Linesheet #175 only) ===
In ReviewPDF.tsx, the Transaction Information section moves to the top of the 
next page, leaving empty space on the previous page. Show how the Transaction 
Information section is rendered — any wrap={false}, break, or minPresenceAhead 
forcing it to the next page?

Show all relevant code across the three files. Confirm whether the memos share 
duplicate code (so a fix applies to both). Do not edit. Findings only.
