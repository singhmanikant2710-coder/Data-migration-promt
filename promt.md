Option A — new dedicated pipeline. Build the CrmFindingsForManagement model + repository + service + controller, following the exact same pattern/folder conventions as the CrmSummaryForManagement backend pipeline (just built). Lean flat query as you described: customerName, reviewId, crmComponent, code, category, comments, filtered on CORE Findings.Finding_level = 'Finding'.

Do NOT reuse the crm-summary-for-management endpoint — I want this report decoupled from CRM Summary for Management's data shape, both for performance (avoid fetching unused scorecard/policy-exception/PD data) and to avoid future coupling/regression risk if that report's shape changes.

Show the backend diff (model + repository + service + controller) before wiring it into the frontend service call. Do NOT commit.
