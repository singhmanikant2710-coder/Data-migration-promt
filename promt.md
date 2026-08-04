READ-ONLY. Diagnostics only. Do not change anything.

AUDIT: We found that the CRM PD Grade Migration report has a hardcoded 
"Approved Only" requirement (@ApprovedOnly defaults to true, forcing 
Review_approval_date IS NOT NULL). The client wants to know if the SAME issue 
exists in OTHER reports run from the Reports Home screen — especially the 
other "CRM" reports.

Search ALL report repositories/services (backend/src/Casrr.Infrastructure/
SqlServer/*ReportRepository.cs and backend/src/Casrr.Application/Reporting/**) 
and report which reports have any of the following:

1. An "ApprovedOnly" parameter/predicate (like @ApprovedOnly defaulting to 
   true, or a WHERE condition forcing Review_approval_date IS NOT NULL).

2. Any WHERE clause that requires Review_approval_date to be populated 
   (Review_approval_date IS NOT NULL, or an INNER JOIN / condition that 
   implicitly drops reviews without an approval date).

3. Whether each report accepts and applies the Reports Home ReviewStatus 
   filter (uses SqlReviewStatusHelper.ReviewStatusPredicate or has a 
   ReviewStatus field), OR ignores status like PD Grade Migration does.

For EACH report found under Reporting, produce a simple table:
  Report name | File | Has ApprovedOnly / approval-date requirement? (Y/N) | 
  Applies ReviewStatus filter? (Y/N) | Notes

Focus especially on the "CRM" reports (CRM Summary, CRM Summary Table, 
CRM Findings, Initial Memo, Final Memo, CAS Linesheet, and any others listed 
in the Reports Home dropdown / Selections library).

Do not edit anything. Just audit and list the findings.
