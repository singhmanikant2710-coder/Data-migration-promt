Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break. Change these two reports only, never shared PDF defaults.

TASK (UAT #215 items 3 and 6, Geoff) — the formatting items are already done;
only add a Commitment column:
(3) CRM Findings and Observations PDF: add "COMMITMENT" column between
    "CUSTOMER NAME (REVIEW ID)" and "SEVERITY".
(6) CRM Findings for Management PDF: add "COMMITMENT" column between
    "CUSTOMER NAME (REVIEW ID)" and "COMMENTS".
Value: the review's commitment exposure. In Phase 1 show which source the
app already uses for review exposure (e.g. Review Queue "Exposure" =
AccountsCommittedExposure / sum of 02_CORE_04_Accounts.Commitment, vs
TBA_exposure) and STOP to ask me with A/B/C options if not obvious.
Format like the Review Queue Exposure column ($ with thousands separators,
no decimals); NULL → "--". Same header style as neighbours. Comments column
gets narrower — text must still wrap, no overflow.

PHASE 1 (read-only): file:line of both PDF components, their report
repositories/SQL/DTOs, column widths (flexBasis %), exposure source.
PHASE 2: add the value to the existing report query/DTO + one column per
report; reuse existing currency formatter. No schema change; other reports
sharing the repository/DTO unaffected.
PHASE 3: implement.
PHASE 4: build backend + frontend; render both PDFs with real data. Do NOT
leave harness/test files in the repo (delete any temp folders like .pdfcheck).

Report:
1. What you found (file:line)
2. ADDED lines (file:line)
3. REMOVED / CHANGED lines (file:line)
4. PDF before vs after (columns, widths, page count)
5. Edge cases: commitment NULL/0, very large values, review with many
   findings, long comments wrapping
6. Other reports confirmed unaffected
7. Build results
8. git status (test-data/ and artifacts flagged)
Do not commit or push.
