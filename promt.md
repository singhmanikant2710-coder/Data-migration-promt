READ-ONLY DIAGNOSTIC. Do NOT edit/create/delete any file. Do NOT use the terminal or PowerShell. Read files directly with your file-read tool and report findings only.

CONTEXT: Bug #35 (High) — "The trailing twelve months (TTM) values are not pulling when I enter a NEW month, but they show correctly for PAST months that were already captured." So TTM/rolling computations work for historical months but fail for a newly entered month. Related defects: #31 (after entering June, Interest Coverage TTM / YTD Net C/O $ / Net C/O TTM % / Reserve Coverage don't populate for the month), #30 (cash collections % and 60+ DPD % not captured after input), #29 (Sept 30 fiscal year-end: YTD revenue/PBT/interest coverage TTM not captured). Likely one shared root cause in how TTM/YTD is computed or how the rolling window is assembled at the moment of a new-month save/read.

TASK: Trace the compute + data-assembly path for TTM/YTD metrics for a freshly entered month, and report ONLY (quote exact file paths + code snippets):

1) TTM COMPUTE LOGIC:
   - In frontend/src/blackbook/components/monthSummaryRegistry.ts: quote computeFccTtmFromRollingWindow(...) and any function that builds a trailing-12 window from a given monthKey (e.g. how it selects the previous 12 months from rolling24). Report exactly how it matches months — does it require the target monthKey to already EXIST in the rolling24 array? Does it look backward only? What happens if the new month is the latest and not yet in rolling24?
   - Quote fccTtmStrict, interestCoverageTtmStrict, computeFccTtm and how they decide between a direct field vs. computing from the window.

2) BACKEND TTM/YTD:
   - In backend/src/Bcat.Application/Services/Calculations/TblMainCalcs.cs: quote the functions computing TTM (trailing twelve) and YTD sums (e.g. perNetChargeOffTTM, YTD revenue/PBT, interest coverage TTM, reserve coverage). Report how they gather the 12-month / year-to-date window — from a passed-in list of rows, or a single row? Do they depend on prior months being present?
   - Report whether YTD resets on fiscal year-end and how fiscal-year-start is determined (relevant to #29's Sept 30 year-end).

3) NEW-MONTH SAVE/READ PATH:
   - In backend/src/Bcat.Api/Controllers/ (BlackBookEditController / MainController / MetricsController) and the matching service: quote the save path for a new month's values, and the read path that returns metrics right after. After a new month is saved, is the rolling window / TTM recomputed and returned, or does it only recompute on a later fetch? Report whether the newly-saved month is included in the rolling24 set used for TTM at read time.

4) FRONTEND ASSEMBLY AFTER ENTRY:
   - In frontend/src/app/blackbook/edit/page.tsx (or customer/edit): report how, after entering a new month, the code refetches or updates rolling24 / series before recomputing columns. Does it include the just-entered month in the array passed to buildMonthSummaryColumns? Quote the state-update / refetch logic.

OUTPUT:
- A) The compute path chain for a new month's TTM/YTD: input rows -> window builder -> TTM/YTD function -> displayed cell.
- B) The single most likely reason TTM/YTD works for past months but NOT for a newly entered month, with file path + quoted code. (Common candidates: the new month isn't in the rolling window at compute time; window builder requires the month to pre-exist; monthKey string/int mismatch; fiscal-year boundary; recompute not triggered after save.)
- C) Ranked secondary suspects with path + snippet.
- D) State whether the root cause is shared with #29/#30/#31 (yes/no + why), based on evidence.
- E) No fix proposed. Findings only.
