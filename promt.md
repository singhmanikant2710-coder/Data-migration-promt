Hi John, Jacob — update on the calculated-field review and your earlier questions.

Calculated fields
We pulled all 35 calculated-field expressions directly from tblMain and compared each one against the new application. Most match exactly, including curEBIT and curEBITTTM (stored values equal Profit Before Taxes + Interest Expense on all 13,835 rows) and elapsed fiscal days. We found a few differences, which we'll correct to match Access:
• Fixed charge coverage (monthly and TTM): one calculation path adds distributions to fixed charges. Access uses CPLTD + Interest Expense only.
• Interest coverage and debt / tangible net worth: one path divides by the absolute value, so a negative denominator (for example, negative tangible net worth) shows a positive ratio. Access keeps the sign.
• perNetIncomeYTDDividedByRevenueYTD: Access uses Profit Before Taxes YTD as the numerator; the new application uses Net Income YTD.
• Minor rounding/truncation differences in AR turn days and inventory turn.
We'll share the final list with before/after examples once fixed.

TTM
We found the legacy recalculation routine (funSave in the industry forms). It confirms what you described: TTM is a calendar 12-month window independent of fiscal year, it is recalculated for the following months after a save, and the TTM components are calculated before the ratios that use them. The new application currently limits TTM to the fiscal year and only recalculates the edited month. We're fixing both. For customers with fewer than 12 months of history, legacy sums the months available; we'll confirm this against an early customer's data as you suggested.

datFiscalYearStart (your question on legacy cleanup)
No cleanup is needed. The new application matches how Access has stored this field since 2020 (36 of 36 rows identical in Access and SQL Server). Older pre-2020 rows follow an earlier convention, and nothing reads this field, so we recommend leaving them as they are.

Two questions
1. John — Keystone Private Income Fund: tblCustomer shows an October fiscal start, but none of its 62 rows match either fiscal-year labelling pattern, while the other three exception customers consistently use the starting-year pattern. Could you check how Keystone's fiscal years were set up before it's included in the cleanup?
2. Jacob — perNetIncomeYTDDividedByRevenueYTD uses Profit Before Taxes YTD in Access, despite the name. We plan to match Access unless it should use Net Income YTD.

Neither question blocks our current fixes.


SELECT TOP 24 strMonthKey, intFiscalYear, intFiscalMonth, datFiscalYearStart
FROM tblMain WHERE strCustomerName = 'KEYSTONE PRIVATE INCOME FUND'
ORDER BY strMonthKey;

SELECT m.strCustomerName, m.strMonthKey, m.intFiscalYear, m.intFiscalMonth
FROM tblMain m
JOIN tblCustomer c ON c.strCustomerName = m.strCustomerName
CROSS APPLY (SELECT CAST(LEFT(m.strMonthKey,4) AS int) AS y,
                    CAST(RIGHT(m.strMonthKey,2) AS int) AS mo) x
WHERE m.strCustomerName IN ('BANKERS HEALTHCARE GROUP LLC','NATIONWIDE SPECIALTY FINANCE INC')
  AND m.intFiscalYear = CASE WHEN x.mo >= c.intFiscalYearMonthStart THEN x.y + 1 ELSE x.y END;


  READ-ONLY. Your last report mixed items 2-4: the code under
"perReserveCoverage" is perInterestCoverageTTM, and the table columns
are shifted (Math.Abs is L3, +Distributions is L4). Re-quote verbatim,
one item per heading, file:line:

1. perReserveCoverage — every computation site, all layers.
2. perIneligiblePercent — every computation site, all layers.
3. Rebuild the MISMATCH rows only. Each cell must cite file:line.
4. Is AccessMainRepository registered in DI for any environment?
   Quote Program.cs / DI setup and appsettings*.json that select it.
5. Quote the SQL of legacy query qryMainYTDCalculations_008perInventoryTurn
   and compare with the app's perInventoryTurn.

Report only.
