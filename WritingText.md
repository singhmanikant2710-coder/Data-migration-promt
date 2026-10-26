Hi Geoff,

Before I build #244 (Record Retention), a few rules to confirm since deletes are permanent:

1. Date parameter: I propose a single date – delete Samples where ALL reviews were finalized before that date. The date must be on or before TODAY minus 7 years (a review finalized exactly on the cutoff date is retained). OK, or do you need a From/To range?
2. If a Sample has any review with no Finalized date (including Cancelled reviews), I propose EXCLUDING that Sample and showing it in the preview with the reason. OK?
3. Samples with zero reviews, and Samples that are not Closed, would also be excluded (shown with reason). OK?
4. "No more frequently than quarterly": I propose showing a warning with the last run date rather than hard-blocking. OK?
5. Audit: I will add a small audit table (via DBA script) recording who ran it, when, the cutoff used, and Sample IDs/names and row counts deleted – no customer names. OK?
6. Impact to be aware of: after a purge, deleted reviews no longer appear in reports, and a retained review whose "Prior CAS Review" was purged will show the next older surviving review (or none).
7. To keep each run short, one run will delete at most 100 Samples; larger volumes need more than one run.
8. Can the DBA team confirm a backup / point-in-time restore is available before the first Prod run?

Thanks,
Manikant
