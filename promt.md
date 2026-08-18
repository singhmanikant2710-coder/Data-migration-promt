READ-ONLY. Diagnostics only. Do NOT change anything. Do NOT modify data.

Files (read ONCE each, findings only, no edits):
- backend/src/Casrr.Infrastructure/SqlServer/SqlScorecardResultsReportRepository.cs
- backend/src/Casrr.Application/Reporting/ScorecardResults/ScorecardResultsReportService.cs
- frontend/src/components/pdf/ScorecardResultsPDF.tsx

Geoff confirmed the de-dup rules:
- Uniqueness key: Review_id + Scorecard_id_bank
- De-dupe at Review_id + Scorecard_id_bank + PD/LGD combo (so identical rows collapse, but same Bank ID with DIFFERENT PD/LGD stays visible to reveal bad data)
- Applies to BOTH the summary count (Item 3) and the details table (Item 4), using the same de-duped population so totals reconcile with rows.

Show me ONLY (no edits):

1. PopulateScorecardsAsync — the FULL current SQL (SELECT + FROM + WHERE + ORDER BY), with EVERY column selected. I need exact column names for Review_id, Scorecard_id_bank, Bank_PD, Bank_LGD, CAS_PD, CAS_LGD, Scorecard_assessment, and any transaction/date columns.

2. ComputeTotalsAsync — the FULL current SQL (the COUNT(*) ... GROUP BY Scorecard_assessment).

3. In the service layer (ScorecardResultsReportService): is there any post-processing between the repo and the response, or does it pass repo rows straight through? Show where totals and detail rows are assembled.

4. In ScorecardResultsPDF.tsx buildGroups: confirm it just renders whatever rows come from backend (no dedup) — so if I dedup in the backend, both the count and details will reflect it. Show how count/totals are consumed.

5. Confirm the exact WHERE/scope: results are already filtered by Review_id IN (...). So de-dup needs to be WITHIN that set, keyed by Review_id + Scorecard_id_bank + PD/LGD.

Read once. Findings only. No edits.
