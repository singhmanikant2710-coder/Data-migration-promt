Confirmed correct — the Selection entry hasn't been added yet, which is why it won't appear in the dropdown. Before I can test this end-to-end, I need:

1. Show me the exact insert-crm-findings-for-management-selection.sql script (idempotent, cloned from whatever pattern was used for CRM Summary for Management's own Selection insert).
2. Confirm: what Selection_id will this get, and does it need to be a specific number, or does the table auto-assign one? Check how CRM Summary for Management's own Selection insert handled this (was it a specific ID, or an auto-increment/MAX+1 pattern?).
3. Also complete the remaining pending items: runCrmFindingsForManagement() in services/api/reporting.ts, the page.tsx routing entries (toReportId + isCrmFindingsForManagement guard + dispatch), and the PDF component's grouped table body using the flat row type (CrmFindingsForManagementRow) the new endpoint actually returns.

Show me the SQL script first — I'll run it myself against the DB (I don't want the agent running inserts directly). Then show the remaining diffs together. Do NOT commit anything.
