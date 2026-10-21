UAT #252 – CRO Production report: monthly columns ignore the year when the date range crosses a calendar/fiscal year

ISSUE (from UAT)
Reporting > CRO Production, Review Status = Completed, "Summary by Reviewer (Monthly Counts)":
- Start 7/1/2025, End 6/30/2026 → columns show JAN 2025 … DEC 2025 and every month has values. Values are probably Jan–Jun 2026 + Jul–Dec 2025 merged by month number, but headers say 2025.
- Start 7/1/2026, End 9/30/2026 → 12 columns JAN 2026 … DEC 2026, values only in JUL–SEP.
Business expectation: columns follow the selected range in chronological order (e.g. JUL 2025 … JUN 2026). Showing only the months in the range is preferred; a fixed 12 columns is acceptable only if months/years are correct.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- Find the CRO Production report end to end with file:line: frontend page/component, API route, backend service/repository, SQL.
- How are monthly buckets computed: MONTH() only, or year+month? Where are the 12 headers generated and which year do they use?
- Which outputs render this table: on-screen preview, Generate PDF Report, Export Report (Excel/CSV)? Do they share the same data/headers code?
- Which date column is filtered and bucketed for each Review Status option? Confirm the filter itself respects start/end correctly.
- Do other reports use the same monthly-bucket helper? List them — they must not break.
- State the exact root cause with file:line.

PHASE 2 – PLAN
- Smallest generic fix: bucket by year+month; build columns from the start month to the end month of the selected range, chronologically, labeled MON YYYY. TOTALS = sum of the shown columns and must equal the count of reviews in the range.
- Same columns in screen, PDF and Export.
- Range longer than 12 months: report how the current layout/PDF width would handle it and propose a safe option (do NOT pick one silently — list options for me).
- No date parameters / only one date given: keep current behaviour unless clearly broken; report what happens.
- Do not change other reports unless they share the same bug through the same helper — if so, list them and ask before changing.
- No schema change. SQL only as idempotent scripts/sql/ if needed (none expected).

PHASE 3 – IMPLEMENT
- Touch only the code needed.

PHASE 4 – VERIFY + REPORT
- Root cause (file:line).
- ADDED (file:line), REMOVED/CHANGED (file:line).
- On-screen before/after for: 7/1/2025–6/30/2026; 7/1/2026–9/30/2026; 1/1/2026–12/31/2026; a single month; a range ending in the middle of a month.
- NULL/edge cases: reviews with NULL dates for the selected status; reviewer with no reviews in range; leap year/month-end boundaries; time-of-day on end date (end date must be inclusive); timezone shifts at month boundaries.
- Confirm TOTALS per reviewer match a plain SQL count for the same range (include that SQL as a read-only check).
- Confirm PDF and Export match the screen; other reports unaffected.
- dotnet build, npx tsc --noEmit, npm run build results.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo if rewritten; leave untracked scripts/sql/find-reviews-with-blanked-ratings.sql alone).
- Do not commit or push.
