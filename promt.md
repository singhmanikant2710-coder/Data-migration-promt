READ-ONLY. No edits, no terminal. Read files/data and report only. Goal: decide between two competing root causes for "TTM not pulling for a newly entered month" — (iii) enrichedSeries render race, vs (B-alt) the new month's row lacks the monthly component fields that computeFccTtmFromRollingWindow needs.

Do these checks and quote evidence:

1) computeFccTtmFromRollingWindow INPUT FIELDS:
   - In monthSummaryRegistry.ts quote fccMonthlyAliases (cafc, fixed, ebitda, interest) — the exact alias keys it sums across the 12-month window.
   - Confirm: does it read MONTHLY component fields (per-month) from each window row, NOT a precomputed *TTM field? Quote the sum() calls.

2) WHAT THE NEW-MONTH ROW ACTUALLY CONTAINS:
   - In blackbook/edit/page.tsx handleAddNewMonth, quote the exact list of field keys written when copyPrev is OFF (the "core" seed array) and confirm whether any of fccMonthlyAliases (cafc/fixed/ebitda/interest monthly keys) are among them.
   - When copyPrev is ON, confirm via the exclusion regex whether the monthly component fields survive the copy (they are NOT ytd/ttm, so they may pass) — list which fccMonthlyAliases keys would be copied vs dropped by the filters (the /(ytd|ttm)/, /(ratio|coverage|availability|percent)/, non-updateable currency set incl. ebitda/cafc). Specifically: is "ebitda"/"curEBITDA" and any CAFC monthly field DROPPED by the non-updateable set? Quote that set again and cross-check against fccMonthlyAliases.

3) DOES THE ROLLING24 ROW FOR THE NEW MONTH CARRY MONTHLY COMPONENTS?
   - The rolling24 endpoint returns tblMain rows. Quote (from AccessMainRepository MapMetricPoint / ProjectRow) whether monthly component columns (the DB columns behind cafc/ebitda/interest/fixed) are mapped into MetricPoint.values. If the new row saved them as 0/absent, the window sum for the new month's slot contributes 0.

4) PERSISTENCE vs FLICKER TEST (logic reasoning, quote deps):
   - Quote the enrichSeriesWithCovenants useEffect deps and confirm: after setSeries(new) runs post-save, does that effect re-run and eventually put the new month INTO enrichedSeries? If yes, the enrichedSeries race is only a transient flicker, not a persistent blank. Quote the dep array and the line that sets enrichedSeries = merged including the new month.

OUTPUT:
- A) State definitively: is TTM missing for the new month because (iii) a transient enrichedSeries race that self-heals, or (B-alt) the new month's persisted row genuinely lacks the monthly component fields the TTM window needs? Cite the quoted evidence.
- B) If (B-alt): identify exactly which fccMonthlyAliases fields are absent/zero on a freshly added month, and whether that's because the seed omits them, the copyPrev filter drops them, or the row never had them.
- C) If genuinely BOTH contribute, rank them.
- D) No fix. Findings only.
