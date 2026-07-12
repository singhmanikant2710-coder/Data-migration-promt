UAT #57 — CRM Ratings tab: the five sub-total boxes must COUNT findings from the CRM Findings tab.

I'm attaching a screenshot showing the five sub-total boxes (all currently showing 0):
RISK RECOGNITION FINDINGS | SCORECARD MANAGEMENT FINDINGS | UNDERWRITING FINDINGS | CREDIT SERVICING FINDINGS | LOAN ADMIN FINDINGS

Client requirement:
"The sub-totals boxes are intended to be sub-totals of 'Findings' from the prior CRM Findings tab for each respective CRM component. If there is one Risk Recognition 'Finding' on the CRM Findings tab, the Risk Recognition for the CRM Ratings tab should display 1. These sub-totals are currently summarizing whether or not the user has cited one of the respective CRM Components Unsatisfactory."

CRITICAL — from related item UAT #58, the client clarifies these counts must include only rows where Severity = "Finding", NOT "Observation".

So each box = the number of rows on the CRM Findings tab where:
  - the row's CRM Component matches that box's component, AND
  - the row's Severity = "Finding" (exclude "Observation")

Component → box mapping (confirm against the actual component IDs in code):
  01-Risk Recognition      → RISK RECOGNITION FINDINGS
  02-Scorecard Management  → SCORECARD MANAGEMENT FINDINGS
  03-Underwriting          → UNDERWRITING FINDINGS
  04-Credit Servicing      → CREDIT SERVICING FINDINGS
  05-Loan Administration   → LOAN ADMIN FINDINGS
(Note: 06-Servicing Systems and 07-Data Integrity have no box — and per UAT #55 they are always Observation anyway, so they would never be counted.)

YOUR TASK:
1. FIRST report (read-only, no edits):
   a. Where the five sub-total boxes are rendered and what value they currently read from (exact JSX + the state/field). Confirm they are currently derived from the UNSAT bit fields.
   b. Whether the CRM Findings rows (component + severity) are already available in the same component/hook that renders the CRM Ratings boxes — the hook useCrmFindings appears to expose both `findings` and `ratings`, and it already has a `findingCounts` derived value. Check what `findingCounts` currently computes.
   c. Confirm whether these sub-totals are DISPLAY-ONLY (derived) or whether they are persisted to the DB. This matters — if a count column exists in the DB, clarify whether we should write to it or just display.

2. THEN propose ONE minimal fix so each box shows the live count of Severity = "Finding" rows for its component, updating immediately as the user adds/edits/deletes findings on the CRM Findings tab.

Requirements:
- No new API calls — the findings rows are already in state.
- The counts must react live to unsaved edits on the CRM Findings tab (use the same in-edit state, not just the saved payload).
- The UNSAT checkboxes below must keep working exactly as they do today — do NOT change their save logic.
- Do not change the save path or persisted data unless you find these counts are genuinely persisted, in which case report that and
