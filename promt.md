Bug 197 — change wrap strategy from every-hyphen to a SINGLE break near 24 chars. Modify the SAME helpers we just added/wired, in the SAME 4 files. Full ID preserved (no truncation). Show all diffs before applying.

NEW WRAP RULE (replace the current split("-").join("-\n") behavior):
Insert exactly ONE "\n" into the Scorecard ID, at the LAST hyphen that falls at or before character index 24. Specifically:
- Find all hyphen positions in the string.
- Choose the hyphen with the greatest index i such that i <= 24 (i.e. the last hyphen within the first 24 chars).
- Insert "\n" AFTER that hyphen (keep the hyphen visible at end of line 1).
- If no hyphen exists at/before index 24 (rare), fall back to inserting "\n" at the last hyphen anywhere in the string; if there are no hyphens at all, break after 24 characters.
- Preserve the existing length>12 guard and dash-normalization already in place. Do NOT remove any characters.
Result: the ID renders on at most 2 lines, e.g.
  df6debd1-a53d-4d79-9857-
  2ec57e2d7754

APPLY to the same helpers/sites only:
1. InitialMemoPDF.tsx — softBreakId() helper (lines ~461-468). Update its hyphen-break logic to the new single-break rule. Sites 1043, 1178, 1190 stay calling it.
2. FinalMemoPDF.tsx — softBreakId() helper (lines ~345-352). Same update. Sites 924, 1058, 1070 unchanged.
3. ReviewPDF.tsx — formatScorecardId() (lines 73-84). Update its hyphen→newline logic to the new single-break rule. Sites 1026, 1035, 1297, 1326 stay calling it.
4. CrmSummaryPDF.tsx — softBreakId() helper (lines ~378-384). Same update. Site 565 unchanged.

Rules:
- Full ID preserved, only ONE newline inserted.
- Do NOT touch pageSetup.ts, hyphenation callback, accountWrap/formatAccountNumber, column widths, or any non-Scorecard-ID cell.
- Do NOT change which sites call the helper — only the helper internals.
- List every file + line changed.

After applying, run a quick node check: for '3ecb14bf-c8d7-4594-8a1a-9ec57e2d7754' and 'df6debd1-a53d-4d79-9857-2ec57e2d7754', print the wrapped output and confirm removing "\n" reproduces the original exactly.

Commit: "Fix Bug 197: wrap Scorecard IDs to 2 lines at last hyphen within 24 chars (Initial/Final Memo, CAS Linesheet, CRM Summary)".
