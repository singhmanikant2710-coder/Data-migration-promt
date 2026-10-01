Slice A is NOT fully applied. Your report says 18 files / +90 lines, but
git diff shows only 3 files changed:
  ReportFilters.cs, ICrmPdGradeMigrationContracts.cs,
  SqlCroProductionSummaryReportRepository.cs
The other 15 repository files have no @RelMgrId / @PortMgrId predicates or
bindings. Slice B must not start until this is fixed, otherwise selecting an
ID option would make those 15 reports ignore the RM/PM filter entirely.

1. Re-apply the predicate + binding inserts to all 15 missing repositories,
   exactly as described in your last report (same text, CRLF preserved,
   additive only, no existing line modified).
2. Verify with `git diff --stat` and paste the output: it must show 18 files.
3. Per file, confirm the @RelMgrId/@PortMgrId predicate and binding counts
   match the existing @RelMgr/@PortMgr counts.
4. Build and report the result.

Report ADDED/REMOVED (file:line), behaviour (must stay inert: no ID sent →
identical rows), and the NULL / no-filter case. Do not commit.


Hi Geoff, a concrete example of the ID-less history: Reviews 19035, 19358,
19587 and 19841 have RM "JOHN C WAGNER II" with no Employee ID, while 23
other reviews for the same person carry ID 17436. Once the Reports filters
group by Employee ID, these 4 will appear as a separate name-only option.
If your Friday reload populates the Employee ID on historical reviews
(17436 here), they'll merge automatically into the single
"17436 - WAGNER, JOHN C" option.
