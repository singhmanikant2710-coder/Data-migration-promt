CORRECTION — Part 4G of your last report is refuted by direct evidence.

I queried tblMain directly for Athens Paper, strMonthKey 202110-202409
(FY2022-FY2024) — a range never touched by this week's testing — in BOTH
SQL Server and MS Access. Results identical in both, 36/36 rows: every row
matches new DateTime(fy, fiscalStartMonth, 1) exactly (the committed hunk f
formula). None match DeriveFiscalYearStartDate — that formula is
consistently off by exactly one year across this entire range.

Combined with the 2018 data (which DOES match DeriveFiscalYearStartDate),
this confirms: legacy changed convention at the ~2019-2020 rollout, exactly
as originally theorized. Hunk (f) correctly replicates the CURRENT/recent
legacy convention. DeriveFiscalYearStartDate matches only the OLD
pre-rollout convention and would be a regression against every row from
2020 onward if applied.

Update Part 4G: no code fix needed for the save-path formula. It's already
correct as committed. Only the historical pre-2020 backfill question
(~1,270 rows, per earlier estimate) remains open, and it's a data decision
for John, not a code change — does not block anything on our side.

Do not propose applying DeriveFiscalYearStartDate anywhere. Acknowledge and
update the report.
