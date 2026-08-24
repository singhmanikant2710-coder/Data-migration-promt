READ-ONLY DIAGNOSTIC. Do NOT edit/create/delete. Do NOT use terminal/PowerShell. Read files directly and quote code. Findings only.

CONTEXT: Confirmed via prior tracing that TTM/YTD works for past months but not a newly entered month, likely because rolling24/series passed to buildMonthSummaryColumns don't include the just-saved month at compute time. I need the EXACT save + refetch + state-update logic on the Blackbook edit page to confirm this and locate the precise line.

TASK: Open the Blackbook edit page and its data hooks. Likely files:
- frontend/src/app/blackbook/edit/page.tsx
- any hook/util it imports for fetching (e.g. useBlackbook, fetchRolling24, a metrics client in frontend/src/blackbook/** or frontend/src/lib/**)
- frontend/src/app/customer/edit/page.tsx (only if it also saves month values)

Quote exactly, with file path:

1) SAVE HANDLER: The function that runs when the user saves/enters a new month's values (onSave / handleSave / submit). Show the full body: what it POSTs, and what it does AFTER the save resolves.

2) POST-SAVE REFETCH: After save succeeds, does the code refetch rolling24 and series? Quote the refetch call(s). Report the EXACT endMonthKey / monthKey argument passed to the rolling24 fetch. Is it the newly-entered month, or a stale selectedMonthKey from state that hasn't updated yet?

3) STATE UPDATE ORDER: Quote where setRolling24 / setSeries / setSelectedMonthKey are called relative to the save + refetch. Is there a stale-closure risk (refetch uses old selectedMonthKey because setState hasn't flushed)? Show the useEffect deps or the await ordering.

4) INITIAL FETCH: Quote how rolling24 and series are first fetched on page load (the fetch URL + query params, and the endMonthKey used). This confirms the "works for past months" path so we can compare it to the new-month path.

5) WHERE buildMonthSummaryColumns IS CALLED: Quote the call site on the edit page — which arrays (series, rolling24) are passed, and what triggers recompute (useEffect deps). Confirm whether the just-entered month is in those arrays when recompute fires.

OUTPUT:
- A) The save -> refetch -> setState -> recompute sequence as an ordered list with file:line for each step.
- B) The exact defect point: pick ONE — (i) no refetch after save, (ii) refetch uses stale/wrong endMonthKey, (iii) refetch happens but setState races the recompute, (iv) refetch is correct but the new month row isn't yet committed server-side when read back. Quote the deciding code.
- C) Confirm: at the moment columns are rebuilt after a new-month save, is the new month present in rolling24 AND series? yes/no + evidence.
- D) No fix. Findings only.
