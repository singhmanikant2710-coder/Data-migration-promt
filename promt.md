Context: .NET 8 Clean Architecture backend, CASRR project. I need to find the
exact table and column that backs the "COMMITTED" dollar figure shown on the
"Review Summary for Management" report (e.g. "COMMITTED: $60,094,601" in the
info grid, alongside OUTSTANDING, BANK PD, CAS PD).

Investigate — read-only, don't change anything:
1. Search the codebase for where this report's data is assembled (likely
   SqlCrmSummaryReportRepository, SqlCrmSummaryForManagementRepository, or
   similar — wherever "Committed" / "COMMITTED" appears as a field label or
   property name in the CRM Summary / Review Summary for Management pipeline).
2. Trace it back to the actual SQL SELECT — which table and column name does
   the "Committed" value come from? Is it sourced from
   [01_DATA_01_Data Mart Trial], [02_CORE_02_Reviews], or some other table?
3. Also check if this same value (or a very similarly-named one) is used
   anywhere for "Committed Exposure" specifically — the client wants to
   sub-total this by RM/PM for a data-gap analysis on Data Mart Trial records
   filtered to SourceSystem IN ('ACBS','MWS','IFL').

Report back: the exact table name and exact column name (with correct
spelling/spacing, since this project has spaced column names like "PM Number"
in brackets) that holds this value. Don't write any SQL for me — just tell me
where it comes from so I can query it directly.
