READ-ONLY. Blackbook PDF is INTERMITTENT: sometimes all months are
correct, sometimes YTD Sales and YTD PBT are 0 for some months
(e.g. ATHENS 202504-202509), sometimes rows are missing. DB and the
edit-page UI are always correct. Suspect a race or cache inconsistency
during PDF generation.

Check and quote file:line:
1. Does the Blackbook button wait for ALL series fetches (current-year,
   prior-years, historic-year, rolling24) and any client-side YTD
   backfill/merge to finish before building the PDF? Any loading-state
   gate, Promise.all, or stale closure/state?
2. Are YTD values for prior fiscal years recomputed/merged on the client
   after load? If PDF reads before that, which cells become 0?
3. Server cache: do the 4 metrics endpoints cache independently
   (300s TTL) so the PDF can mix fresh and stale series?
4. Does "0" come from a default/fallback (?? 0, formatCurrency(null))?
Report only. Include how to reproduce reliably.
