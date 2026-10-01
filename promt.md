Decisions:

1) Option A: two slices, options first.
Condition: slice 1 must NOT change which reviews a report returns. While the
predicates are still name-based, selecting an ID option must still produce
the same results as before (e.g. send the stored name variants for that ID
if the predicate supports a list, otherwise keep slice 1 uncommitted until
slice 2 is ready). Never ship a dropdown whose selection filters on a name
that doesn't exist in Reviews.
For slice 2, give me before/after SQL count queries per report (Wagner 17436
filter + no filter), so I can verify in SSMS myself.

2) Option B: make CRO Review Production Summary honour the RM/PM filter,
same predicate as the other 15 reports. List this explicitly in the report
as a behaviour change (row counts drop when an RM/PM filter is set). The
PDF must no longer claim a filter that isn't applied.

3) Option C: prefix with the ID, e.g. "17436 - JOHN C WAGNER II". For the
name part, use the most frequent stored variant for that ID, with
max(Review_id) as the deterministic tiebreak. IDs that ARE in Distribution
Parties use the canonical label (also "ID - NAME"), so all ID options share
one format. Name-only legacy options have no prefix.

Report ADDED/REMOVED (file:line) per slice, the on-screen change, the
NULL / all-zero / no-filter cases, and the build results. Do not commit.


FYI Geoff: while fixing the duplicate RM/PM names in the Reports filters, we
found the CRO Review Production Summary report was ignoring the RM/PM filter
(it showed all rows but printed the filter as applied). We're fixing it to
filter like the other reports, so its row counts will now correctly drop
when an RM/PM filter is selected.
