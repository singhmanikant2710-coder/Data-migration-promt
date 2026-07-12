UAT #58 is not working. Proof (same review, same moment):
- CRM Findings has ONE row: 01-Risk Recognition, RR-101, Severity = "Finding".
- CRM Ratings tab correctly shows RISK RECOGNITION FINDINGS = 1 (the #57 fix works).
- The UNSAT RISK RECOGNITION checkbox is TICKED.
- BUT on Risk Rating Justification: "RISK RECOGNITION KEY FINDINGS" shows 0, and "UNSATISFACTORY RISK RECOGNITION" shows "No".

Both tiles are still reading the old sources.

Show me (read-only, verbatim) the CURRENT JSX of those two StatCards in RiskRatingJustificationSection.tsx — the exact `value=` and `danger=` props of each.

I need to see whether they still read:
  value={String(d.riskRecognitionKeyFindings)}
  value={(d.riskRecognitionRating ?? "").toLowerCase() === "unsatisfactory" ? "Yes" : "No"}
or the new derived values (rrFindingCount / rrUnsat).

Also confirm: was RiskRatingJustificationSection.tsx actually modified, and does it now import useCrmFindings and read changes?.changes?.crmFindingsAndRatings?

If the file was never changed, apply the approved fix now:
- rrFindingCount = count of effectiveFindings where component starts with "01-" AND severity === "Finding"
  (effectiveFindings = Array.isArray(pendingFindings) ? pendingFindings : crmState?.findings ?? [])
- rrUnsat = effective rating (pending ?? saved from useCrmFindings) === "Unsatisfactory"
- Wire both StatCards to these.

Display-only. No save-path changes. No new API calls.

Show me the diff with BEFORE and AFTER of both StatCard props. STOP after applying.
