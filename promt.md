UAT #57 is still broken. Proof: on CRM Findings there is ONE row (01-Risk Recognition, RR-101) whose Severity is now "Observation" — so there are ZERO rows with Severity = "Finding". Yet on CRM Ratings, "RISK RECOGNITION FINDINGS" still shows 1, because the UNSAT RISK RECOGNITION checkbox is ticked.

This proves the five StatCards in CrmRatingsSection.tsx are STILL reading from the UNSAT rating flags, not from the finding counts. That file was evidently never updated.

Fix it now — single file: CrmRatingsSection.tsx.

1. Compute the effective findings in this component:
   const pendingFindings = changes?.changes?.crmFindingsAndRatings?.findings;
   const effectiveFindings = Array.isArray(pendingFindings) ? pendingFindings : (s?.findings ?? []);
   (Use useFormChangesOptional / the same context accessor the other sections use.)

2. Derive DISPLAY-ONLY counts — rows where severity === "Finding", bucketed by component prefix:
   riskRecognition      → component starts with "01-"
   scorecardManagement  → "02-"
   underwriting         → "03-"
   creditServicing      → "04-"
   loanAdministration   → "05-"

3. REPLACE the five StatCard value props. They currently look like:
   value={(s?.ratings.riskRecognition === "Unsatisfactory") ? 1 : 0}
   They must become:
   value={counts.riskRecognition ?? 0}
   ...and the same for the other four.

Hard constraints:
- The UNSAT checkboxes, setRating and the save path must NOT change at all.
- Counts are display-only — never written to state, never in a save payload.
- No new API calls.

Show me the diff of CrmRatingsSection.tsx, and paste the BEFORE and AFTER of all five StatCard value props so I can verify. STOP after applying.
