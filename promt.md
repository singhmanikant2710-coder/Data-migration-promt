READ-ONLY. No edits. Find why UI shows Interest Expense TTM = $0 and PBT TTM = 167000, when tblMainTTMCalculations has curInterestExpenseTTM=387000 and curProfitBeforeTaxesTTM=350000. The merge from tblMainTTMCalculations to the UI is broken or using wrong keys. Quote, stop.

Trace exactly:

1) SqlMainRepository.cs TryMergeTtmIntoSeries: quote the FULL method. I need to see:
   - The exact dict[...] = Read(...) assignments — what KEY names does it store (e.g. dict["InterestExpenseTTM"] vs dict["curInterestExpenseTTM"])?
   - The final loop that writes into p.Values — what key names end up on the row? Quote the loop.
   - Is TryMergeTtmIntoSeries actually CALLED in the read path that feeds the edit page? Quote the call site (which method calls it, e.g. in GetCurrentYearSeries / GetRolling24Months).

2) Whether TryMergeTtmIntoSeries is called for the arrays the UI uses. The UI Month/TTM panel reads from series/seriesWithEdits (current-year) and/or rolling24. Confirm which repository read method feeds those, and whether that method calls TryMergeTtmIntoSeries. If the current-year read does NOT call it, that explains why the merged TTM values never reach the panel.

3) The frontend read for "Interest Expense TTM" and "PBT TTM" in the Month/TTM panel: in monthSummaryProfiles / the panel mapping (mapIndirectAuto or the top-strip/panel builder), quote the exact alias/field it reads for "Interest Expense TTM" and "PBT TTM". Which key does it expect — curInterestExpenseTTM, InterestExpenseTTM, or something else?

4) Cross-check: does the KEY that TryMergeTtmIntoSeries writes match the KEY the frontend panel reads? List both exact strings side by side. Also: does the existing RecomputePbtTtmAsync write curProfitBeforeTaxesTTM into tblMain (=167000) which the frontend reads INSTEAD of the tblMainTTMCalculations value (=350000)? That would explain PBT TTM showing 167000 not 350000.

OUTPUT:
- A) TryMergeTtmIntoSeries key assignments + write loop keys (quoted).
- B) Is it called for the current-year series the UI panel uses? yes/no + quoted call site.
- C) Frontend panel's expected keys for Interest Expense TTM / PBT TTM (quoted).
- D) State plainly the broken link: (i) merge not called for the UI's array, (ii) key-name mismatch between merge output and frontend read, or (iii) frontend reads tblMain columns (0 / stale 167000) instead of merged values. Identify which.
- No fix yet. Findings only.
