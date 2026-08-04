READ-ONLY. Diagnostics only. Do not change anything.

Geoff's comment: "This report [CRM PD Grade Migration] appears to have an 
underlying requirement for [Review_approval_date] to be populated. That 
overrules any status selections from the Reports Home page." He wants the 
report to be approval-date agnostic, so the Reports Home "Status" filter 
drives which reviews appear (not the presence of Review_approval_date).

Investigate (no edits):
1. Find the backend query/repository that fetches the detail rows for the 
   CRM PD Grade Migration report (likely SqlCrmPdGradeMigrationReportRepository.cs 
   or similar). Show the SQL — specifically the WHERE clause. Does it filter 
   on Review_approval_date IS NOT NULL (or require approval_date populated)? 
   Show that predicate.

2. How does the Reports Home "Status" filter reach this query? Show how the 
   report request/filters (status, sampleId, dates) are passed into the 
   repository, and whether a status filter is currently applied at all in 
   this report's query.

3. Is there any join or condition that implicitly requires approval_date 
   (e.g. an INNER JOIN on an approval table, or ORDER BY / filtering that 
   drops rows without approval_date)?

4. Compare: how does the Review Status / Review Queue logic (from issue #163) 
   apply the status filter? Could the same status-bucket approach be reused 
   here so the report respects the Reports Home status selection instead of 
   requiring approval_date?

5. Confirm what "Status" values the Reports Home page can pass (the same 
   buckets as Review Status: Unopened/Cancelled, In Progress, Draft Completed, 
   Approved, Distributed, Finalized?), and whether this report's request 
   object even has a status field.

Report the exact WHERE clause, how status is (or isn't) applied, and what 
would need to change to make the report status-driven instead of 
approval-date-driven. Do NOT edit anything. Findings only.
