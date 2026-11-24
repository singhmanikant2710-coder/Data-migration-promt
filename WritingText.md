UAT #259 (Bug) – Final Memos (Consolidated) report: same Finding/Observation rule as the Review Form Final Memo, and apply all report filters

REQUIREMENT (Geoff)
1. The consolidated report (Reports Home) must use exactly the same Finding/Observation selection as the Final Memo run from the Review Form: CRM Findings plus Servicing Systems / Data Integrity Observations only. Other CRM Observations must be excluded today they are included.
2. Hierarchy, Portfolio and General filters (incl. #262 Approved status, CAS Manager, Approving Manager, Special Assets, CCL) must apply, like the other CRM reports.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- The Review Form Final Memo: where the findings list is selected/filtered (FinalMemoPDF.tsx and its data source/API, file:line) – the exact rule (Finding_level, components 06-Servicing Systems / 07-Data Integrity, CRM-00, ordering).
- The consolidated report path: reports/page.tsx branch, service, repository (CRM Final Memos), which filters it applies today and which it ignores (file:line), and how it renders (reuses FinalMemoPDF?).
- Excel export of this report (if any) – same data?

PHASE 2/3 – PLAN + IMPLEMENT (smallest generic change)
- One shared findings rule used by both the Review Form Final Memo and the consolidated report (no second copy). Review Form Final Memo output must not change.
- Apply the standard filters exactly like the other CRM report repositories (reuse SqlReviewStatusHelper, SqlReportPeopleFilterHelper, existing hierarchy/portfolio/general predicates). Cancelled excluded as today.
- Do NOT decide silently – list any decision (e.g. Review Status with no selection: keep "finalized only" baseline?).

PHASE 4 – VERIFY + REPORT (in chat, not in a file)
- Root cause, ADDED / CHANGED (file:line).
- Before/after for one review: findings shown in the Review Form Final Memo vs the consolidated report (must be identical).
- Filter checks: Segment, Unit, RM, Special Assets, CCL, CAS Manager – read-only SQL giving the expected review count for each, to compare.
- dotnet build + tests, npx tsc --noEmit, npm run build.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo; leave pre-existing SQL scripts and devin-output/ alone).
- Do not commit or push.

- Thanks Geoff – I'll start on #259 and #261. For the Memo/Linesheet formatting item (logged as #262 – note this number is also used for the Reports filters item), I'm first reviewing how each comment field is rendered and how pasted Word/PDF/web text is stored. A short demo would be very helpful – could you share a couple of examples (review IDs) where the formatting looks wrong, and let me know a time that works for you?
