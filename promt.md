Bug 225 follow-up — same row-split-across-page-boundary issue, now reported in the Policy Exception Information table (CrmSummaryPDF.tsx, pages 7/8 and 15/16). Fix that table AND proactively fix Unsatisfactory CRM Ratings and CRM Findings and Observations (Geoff wants confirmation these won't have the same issue). SINGLE FILE, mirror the Bug 225 fix pattern exactly. Show diff, do NOT commit.

FILE: frontend/src/components/pdf/CrmSummaryPDF.tsx

From the earlier Bug 225 investigation, these row <View> elements were identified as missing wrap={false} (same pattern as the already-fixed Scorecard Assessment table at lines 583/597):

1. Policy Exception rows — header (~line 617) and data row (~line 623): append wrap={false} to both.
2. Unsatisfactory rows — header (~line 641) and data row (~line 648): append wrap={false} to both.
3. Also check CRM Findings and Observations section (mentioned by Geoff) — find its header/data row <View> elements in this file and confirm whether they already have wrap={false} or need it too. Add it if missing.

Verify current line numbers first (they may have shifted slightly since the earlier investigation due to the Commitment column changes from Bug 214). Mirror the exact working pattern already used in NonCompliantCovenantsPDF.tsx / CroProductionSummaryPDF.tsx — wrap={false} on the row <View>, not on inner <Text>.

Do NOT touch the Scorecard Assessment table (583/597 — already fixed and confirmed working). Do NOT change column widths or any cell content logic — only add the missing wrap={false} attributes.

Show diff. Rebuild. Do NOT commit.
