Multi-file edit: Make CRM PD Grade Migration status-driven (client-approved). 
Copy the EXACT proven pattern from CRM Sample Summary. Keep the existing 
CrmPdGradeMigrationRequest structure — just ADD status support and remove the 
ApprovedOnly gate. Do NOT switch to ReportExecutionRequest (too broad); 
minimal targeted change only.

=== FILE 1: Request DTO ===
backend/src/Casrr.Application/Reporting/CrmPdGradeMigration/ICrmPdGradeMigrationContracts.cs

In CrmPdGradeMigrationRequest, ADD three fields (mirroring ReportFilters):
    public Casrr.Application.Reporting.Contracts.ReviewStatus? ReviewStatus { get; set; }
    public DateOnly? StartDate { get; set; }
    public DateOnly? EndDate { get; set; }
(Use the correct namespace for the ReviewStatus enum — match how ReportFilters 
references it.)

CHANGE the ApprovedOnly default from true to false (so it no longer forces 
approval by default):
    public bool? ApprovedOnly { get; set; } = false;
(Keep the field so it remains available as an OPTIONAL filter, but off by 
default — per Geoff, status should drive, not approval.)

=== FILE 2: Repository ===
backend/src/Casrr.Infrastructure/SqlServer/SqlCrmPdGradeMigrationReportRepository.cs

a) INJECT the ReviewStatusPredicate into the WHERE clause. Add this line into 
   the WHERE block (the Reviews table alias in this repo is "r", which matches 
   what ReviewStatusPredicate expects):
       AND (@ReviewStatus IS NULL OR ReviewStatusPredicate)
   Actually, match the reference: the predicate itself handles the NULL case 
   (when @ReviewStatus IS NULL it falls back to Completed_date IS NOT NULL). 
   Copy the reference approach exactly: add
       AND ReviewStatusPredicate
   into the WHERE, then after building the sql string, add the replacement:
       sql = sql.Replace(nameof(ReviewStatusPredicate), ReviewStatusPredicate, StringComparison.Ordinal);
   (Reference the SqlReviewStatusHelper.ReviewStatusPredicate constant, same 
   as SqlCrmSampleSummaryReportRepository does.)

b) The existing ApprovedOnly predicate line:
       AND (@ApprovedOnly IS NULL OR @ApprovedOnly = 0 OR r.[Review_approval_date] IS NOT NULL)
   Keep this line AS-IS — since ApprovedOnly now defaults to false, this 
   predicate becomes a no-op by default (only filters when explicitly set to 
   true). Do NOT remove it; just rely on the changed default. Confirm the 
   @ApprovedOnly binding still passes request.ApprovedOnly (which now 
   defaults false).

c) ADD parameter bindings for @ReviewStatus, @StartDate, @EndDate — copy 
   EXACTLY from SqlCrmSampleSummaryReportRepository:
       cmd.Parameters.Add(new SqlParameter("@ReviewStatus", SqlDbType.NVarChar, 50)
       { Value = (object?)request.ReviewStatus?.ToString() ?? DBNull.Value });

       DateTime? start = request.StartDate.HasValue
           ? request.StartDate.Value.ToDateTime(TimeOnly.MinValue) : (DateTime?)null;
       DateTime? end = request.EndDate.HasValue
           ? request.EndDate.Value.ToDateTime(new TimeOnly(23, 59, 59)) : (DateTime?)null;

       cmd.Parameters.Add(new SqlParameter("@StartDate", SqlDbType.DateTime)
       { Value = (object?)start ?? DBNull.Value });
       cmd.Parameters.Add(new SqlParameter("@EndDate", SqlDbType.DateTime)
       { Value = (object?)end ?? DBNull.Value });
   (Adjust "request" vs "filters" to match this repo's variable name for the 
   request object.)

=== FILE 3 & 4: Frontend request type + API call ===
frontend/src/services/api/reporting.ts

a) In CrmPdGradeMigrationRequest (frontend type), ADD:
       reviewStatus?: 'completed' | 'distributed' | 'finalized' | null;
       startDate?: string | null;
       endDate?: string | null;

b) Ensure the Reports Home page passes the selected reviewStatus (and start/
   end dates if present) into the crmPdGradeMigration(req) call. Show where 
   the Reports Home page builds the request for this report and add 
   reviewStatus/startDate/endDate to it (mirroring how it's passed for CRM 
   Sample Summary).

CONSTRAINTS:
- Copy the ReviewStatusPredicate injection, the sql.Replace call, and the 
  three parameter bindings EXACTLY from SqlCrmSampleSummaryReportRepository — 
  do not invent new logic.
- Do NOT change the other existing filters (Segment, Unit, Market, etc.) or 
  the matrices/distributions/direction logic.
- Do NOT switch to ReportExecutionRequest; keep CrmPdGradeMigrationRequest.
- ApprovedOnly stays as an optional field but defaults to false now.
- Show every change: the DTO fields, the WHERE injection + sql.Replace, the 
  three param bindings, and the frontend type + wiring.
