1: B – standard ReviewStatusPredicate (incl. Approved and Start/End window) AND Review_finalized_date IS NOT NULL. No status selected = finalized reviews only, as today.
2: A – run when a Sample OR any filter (hierarchy, portfolio, general, status/dates) is selected; with nothing selected show the existing "select a sample or filter" message, no huge report.
3: A – include r.[Segment] IS NOT NULL.
4: A – pass the applied filters to the Applied/Current Filter page (display only).
Proceed with Phase 3 and 4.
