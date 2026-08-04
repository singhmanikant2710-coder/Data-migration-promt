Audited all the CRM reports run from Reports Home. Good news — the "Approved Only" / approval-date requirement is isolated to the CRM PD Grade Migration report only (the one Shivam built). All the other CRM reports (Summary, Summary Table, Findings, Sample Summary, Scorecard Results) already use the standard status-driven logic and respect the Reports Home Status filter — they're clean. The memos are single-review/finalized-specific and aren't affected. So it's just the one report to fix, as you suspected. I'll wire PD Grade Migration to the same status-driven pattern the other CRM reports already use.


READ-ONLY. Diagnostics only. Do not change anything. Focused — do not 
wander, just show one reference file's pattern.

I'm making CRM PD Grade Migration status-driven by copying the pattern from 
a CRM report that already works. Use SqlCrmSampleSummaryReportRepository.cs 
(or CRM Summary if simpler) as the reference. Show me VERBATIM:

1. The WHERE clause section showing how ReviewStatusPredicate is injected 
   (the exact line, e.g. AND (ReviewStatusPredicate) and the sql.Replace(...) 
   call that swaps it in).

2. SqlReviewStatusHelper.ReviewStatusPredicate — the exact predicate string 
   and which parameters it needs (@ReviewStatus, @StartDate, @EndDate) and 
   which date columns it references.

3. The exact parameter bindings for @ReviewStatus, @StartDate, @EndDate in 
   that reference repository (types, how enum/null is bound).

4. The request DTO / ReportFilters that carries ReviewStatus into that report, 
   and how the frontend sends reviewStatus + dates for it (the API call shape).

Just show these four things from the reference report verbatim. No table, no 
analysis of other reports. Findings only.
