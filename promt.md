Bug 214 issue 2 fix — 2 decimal places on the 4 percentage columns. SINGLE FILE. Show diff, do NOT commit.

FILE: frontend/src/components/pdf/CrmSummaryTablePDF.tsx

1. Lines ~343, ~346 (countPct, expPct): change the hardcoded pct01(frac, 0) to pct01(frac, 2) in both functions.
2. Line ~337 (the Intl.NumberFormat call inside pct01): add minimumFractionDigits: maxFrac alongside maximumFractionDigits: maxFrac, so both are set to the same value (fixed 2 decimals, not just an upper bound). Also apply the same fix to the catch-fallback (line 339, .toFixed(maxFrac) already handles this correctly, no change needed there).

Verify: 0 → "0.00%", 0.03 → "3.00%", 0.0344827 → "3.45%". Mirror the correct pattern already used in ManagementSummaryPDF.tsx (198-199) / pageSetup.ts (405-406).

Do NOT change pct01's default parameter value — the fix must be at the two call sites (343, 346) plus the minimumFractionDigits addition, since defaults are irrelevant when call sites pass explicit values.

Show diff. Rebuild. Do NOT commit.


Hi Geoff, on the Commitment column request for CRM Summary Table detail tables — a quick clarification needed. The detail tables show one row per finding, so a borrower with 2 findings appears on 2 rows. If we show that borrower's full Commitment on every row, the number will visually repeat and anyone summing the column would get a doubled total (the same issue as the Exposure bug you reported).
Which would you prefer:
A) Show Commitment only on the borrower's first row, blank on subsequent finding rows for the same borrower (cleanest — no risk of double-counting if summed)
B) Repeat the full Commitment on every finding row (simplest, but a naive column sum would double-count)
C) Add a section subtotal at the bottom (distinct borrowers only)
My recommendation is A. Let me know and I'll implement it — this also needs a small backend addition (Commitment isn't in the details query today) so I want to get the design right the first time.
