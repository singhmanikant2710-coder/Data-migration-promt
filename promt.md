UAT #58 — Risk Rating Justification tab: sub-totals.

Client requirement (two parts):
(a) "The Risk Recognition Findings sub-total field should count the number of 01-Risk Recognition 'Findings' (not Observations) — similar to Item 57."
(b) "The Unsatisfactory Risk Recognition sub-total should show 'Yes' if Risk_recognition_UNSAT = Yes/True. This is not currently working."

CONTEXT — UAT #57 is DONE and this reuses the exact same pattern. In CrmRatingsSection.tsx we now do:
  const pendingFindings = changes?.changes?.crmFindingsAndRatings?.findings;
  const effectiveFindings = Array.isArray(pendingFindings) ? pendingFindings : (s?.findings ?? []);
  // then count rows where severity === "Finding", bucketed by component prefix ("01-", "02-", ...)
FormChangesContext has also been fixed to keep arrays as real arrays (mergeValues), so Array.isArray works correctly.

YOUR TASK:
1. FIRST report (read-only, no edits):
   a. In RiskRatingJustificationSection.tsx: show the exact JSX of BOTH sub-total fields — the "Risk Recognition Findings" one and the "Unsatisfactory Risk Recognition" one — and what value each currently reads from.
   b. For (b): trace where Risk_recognition_UNSAT comes from end-to-end — which state field / DTO / API property holds it, and why the sub-total is not showing "Yes" today. Is it reading the wrong field, a type mismatch (bit vs boolean vs string), or is the value simply not being read at all? Prove it.
   c. Confirm the CRM findings rows and the UNSAT rating flags are reachable from this component (via useCrmFindings / FormChangesContext) without any new API call.

2. THEN propose ONE minimal fix:
   - (a) Risk Recognition Findings sub-total = count of effectiveFindings where component starts with "01-" AND severity === "Finding". Must react live to unsaved edits on the CRM Findings tab, exactly like the #57 StatCards.
   - (b) Unsatisfactory Risk Recognition sub-total shows "Yes" when the Risk_recognition_UNSAT flag is true/Yes, otherwise "No" (confirm with me what it should show when false — "No" or blank).

Hard constraints:
- DISPLAY-ONLY: no writes to state, no changes to any save payload, no new API calls.
- Do NOT touch the save path or the UNSAT checkboxes on the CRM Ratings tab.
- Must work with unsaved (pending) edits, not just saved data.

Report findings and plan with exact files touched. STOP and wait for approval.
