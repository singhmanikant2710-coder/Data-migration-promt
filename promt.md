Bug 197 fix — force Scorecard ID to wrap at hyphens in PDF table cells. Use the EXISTING proven approach already in this repo (softBreakId in ScorecardResultsPDF.tsx, which does s.split("-").join("-\n") and does NOT have this bug). Do NOT rely on wordBreak (unsupported in @react-pdf v4). Do NOT truncate the ID. Do NOT change the hyphenation callback or any shared pageSetup.ts. Show all diffs before applying.

Scope: FOUR files, only the Scorecard ID render sites listed. Do not touch other cells, columns, account-number wrapping, or unrelated styles.

For each file, make the Scorecard ID value pass through a hyphen-breaking formatter that inserts "\n" after each hyphen (mirror ScorecardResultsPDF.tsx softBreakId exactly). Apply it ONLY to the scorecard ID values at these lines:

1. InitialMemoPDF.tsx — line 1038 (scorecardIdBank), line 1185 (scorecardIdBank in Account Info). The existing hyphenWrap() here does nothing useful for IDs; either fix hyphenWrap to insert "-\n" after hyphens OR wrap the ID value with a local softBreakId helper. Keep dash-normalization behavior. Do NOT change accountWrap.
2. FinalMemoPDF.tsx — line 919, line 1053, line 1065. Same treatment.
3. ReviewPDF.tsx (CAS Linesheet) — lines 1026, 1035, 1297, 1326. Note formatScorecardId already exists here (lines 73-84) and does the hyphen→newline transform but is NEVER called. Simply call the existing formatScorecardId on the scorecard ID values at these render sites. Do not add a new helper if this one works.
4. CrmSummaryPDF.tsx — line 558. The value is bare out(). Wrap it with a softBreakId-style hyphen-break helper.

Rules:
- Preserve the full ID (no characters removed). Only newlines inserted after hyphens.
- Keep existing styles; you may leave the (no-op) wordBreak declarations as-is or remove them, but do not add unsupported styles.
- Do NOT modify pageSetup.ts, the hyphenation callback, account-number formatting, column widths, or any non-Scorecard-ID cell.
- List every file and line changed.

Commit: "Fix Bug 197: wrap Scorecard IDs at hyphens in Initial/Final Memo, CAS Linesheet, CRM Summary PDFs".
