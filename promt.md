Hold before applying. Before I approve, I need to verify DeriveFiscalYearStartDate's
CORE FORMULA — not just the null-guard you just added.

The function body is:
    return new DateTime(y, m, 1).AddMonths(-(fiscalMonth - 1));

This uses the RAW CALENDAR YEAR (y, parsed directly from the month key) — not fy
(the fiscal-year label we derive elsewhere).

Concrete test case: Athens Paper Company Inc, monthKey 202510, fiscalMonth=1.
- y=2025, m=10, fiscalMonth=1 → AddMonths(-(1-1)) = AddMonths(0) → returns 2025-10-01.

But yesterday we verified DIRECTLY AGAINST LEGACY ACCESS that the correct
datFiscalYearStart for this exact row (Athens 202510) is 2026-10-01 — using fy
(2026, the fiscal year label), not the raw calendar year (2025). That confirmed
result was the entire basis for yesterday's approved fix (hunk f):
    new DateTime(fy, fiscalStartMonth.Value, 1)

So your new formula and yesterday's approved formula produce DIFFERENT answers
for the same input, and I've confirmed which one legacy actually uses.

Before I approve anything, answer:
1. Is DeriveFiscalYearStartDate used anywhere to write the datFiscalYearStart
   column in tblMain?
2. If yes — walk through why it uses calendar year (y) instead of fy, and
   explain why that doesn't contradict the legacy-verified Athens 202510 result
   from yesterday.
3. Regardless of your answer to #2, show me explicitly: what does this function
   return for Athens 202510 (monthKey=202510, fiscalMonth=1)? If it's 2025-10-01
   instead of the confirmed-correct 2026-10-01, that's a regression and needs
   to be fixed before anything is applied.

Do NOT apply anything — including the null-guard — until this is resolved. If
the formula does need to change, resubmit the full corrected diff for review,
don't apply partially.
