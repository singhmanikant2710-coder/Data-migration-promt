Hi Geoff,

Before I build #244 (Record Retention), a few rules to confirm since deletes are permanent:

1. Date parameters: should the Admin enter a single cutoff date (delete Samples where ALL reviews were finalized on or before it), or a From/To range? In both cases the date must be on or before TODAY minus 7 years.
2. If a Sample has any review with no Finalized date (e.g. Cancelled or never finalized), should that Sample be EXCLUDED from deletion? (My recommendation: exclude and show it in the preview with the reason.)
3. Should a Sample with zero reviews be included?
4. "No more frequently than quarterly": should the system block a run within 90 days of the last one, or just show a warning?
5. Audit: may I add a small audit table recording who ran it, when, the cutoff used, and the Sample names/IDs and row counts deleted? (Ashok/John would create it via script.)
6. Prod run: can the DBA team confirm a backup / point-in-time restore is available before the first Prod run?

Thanks,
Manikant
