Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break. Change these two reports only, never shared PDF defaults.

TASK (UAT #215, Geoff) — CRM Findings and Observations PDF + CRM Findings for
Management PDF.

Already done in earlier work — VERIFY only, change nothing unless broken:
(1) Findings & Observations header height + date font match Findings for
Mgmt; (2) no "01-"/"##-" prefix in component titles; (4) table header colour
matches Findings for Mgmt (navy); (5) Findings for Mgmt has no hyphen before
"(1)". Report file:line for each with PASS/FAIL.

NEW work:
(3) CRM Findings and Observations: add a "Commitment" column between
    "Customer Name (Review ID)" and "Severity".
(6) CRM Findings for Management: add a "Commitment" column between
    "Customer Name (Review ID)" and "Comments".
Value: the review's commitment exposure. In Phase 1 show which source the
app already uses for review exposure (e.g. Review Queue "Exposure" =
AccountsCommittedExposure / sum of 02_CORE_04_Accounts.Commitment, vs
TBA_exposure) and STOP to ask me with A/B/C options if it's not obvious.
Format like the Review Queue Exposure column ($ with thousands separators,
no decimals); NULL → "--". Same value on every finding row of that review.
Header text exactly "COMMITMENT" (same header style as neighbours).
Comments column gets narrower — keep text wrapping, no overflow.

PHASE 1 (read-only): file:line of both PDF components, their report
repositories/SQL/DTOs, column widths (flexBasis %), and the exposure source.
PHASE 2: smallest change: add the value to the existing report query/DTO
and one column per report; reuse existing currency formatter. No schema
change; other reports sharing the repository/DTO must be unaffected.
PHASE 3: implement.
PHASE 4: build backend + frontend; render both PDFs with real data.

Report:
1. What you found (file:line) + PASS/FAIL for items 1, 2, 4, 5
2. ADDED lines (file:line)
3. REMOVED / CHANGED lines (file:line)
4. PDF before vs after (columns, widths, page count)
5. Edge cases: commitment NULL/0, very large values, review with many
   findings, long comments wrapping, landscape fit
6. Other reports confirmed unaffected
7. Build results
8. git status (test-data/ and artifacts flagged)
Do not commit or push.
