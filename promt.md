READ-ONLY DIAGNOSTIC. Do NOT edit. Report only.

The Review Queue "Draft Completed" table shows real Exposure values (e.g. 
$17,581,883), but Review Status (and Review History) show Exposure as $0. 
Find out why.

1. In the Review Queue backend (SqlReviewRepository.cs / GetQueuePageAsync), 
   which exact DB column does it read for "Exposure"? Paste the column name and 
   how it's selected/mapped.

2. In the Review Status backend (SqlReviewStatusRepository.cs) — for the 
   "Completed Draft Reviews" table — which column does it read for Exposure? 
   Paste it.

3. In the Review History backend (SqlReviewHistoryRepository.cs) — which column 
   does it read for Exposure (it currently selects r.[TTBA_exposure])? Paste it.

4. Compare: does Queue use a DIFFERENT column than Status/History for exposure? 
   List the exact column name each one uses.

5. If possible, tell me: in 02_CORE_02_Reviews, is there more than one 
   exposure-related column (e.g. TTBA_exposure vs another exposure column), and 
   which one actually holds the dollar values that Queue displays?

Report only. No edits.
