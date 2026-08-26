SINGLE-FILE, ADD-ONLY, BOUNDED EDIT. Only edit backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs. Do NOT remove, rename, or modify any existing method or line. ADD only: one new method + one new call. Show the unified diff BEFORE applying. Do not run terminal/build. If anything needs changing existing code, STOP and ask.

GOAL (client-approved): At save time, populate dbo.tblMainTTMCalculations for the saved month with trailing-12-month TTM values, year-agnostic (current month + previous 11, crossing fiscal-year boundaries, NO fiscal-year filter). Where a source has no value the TTM stays NULL (no COALESCE on outputs). The read path TryMergeTtmIntoSeries already reads this table.

CONFIRMED: target table dbo.tblMainTTMCalculations has composite PK (strCustomerName, strMonthKey). Source monthly columns exist in dbo.tblMain: curInterestExpense, curProfitBeforeTaxes, curDepreciation, curAmortization, curDistributions, curCPLTD, curNetChargeOff, curPrincipalNR, curAverageGrossNR. strMonthKey is YYYYMM (string ORDER BY = chronological).

STEP 1 — ADD this method immediately AFTER the existing RecomputePbtTtmAsync (do not modify RecomputePbtTtmAsync). Use EXACTLY this method body (do not add/duplicate columns, do not add spaces in aliases, do not add COALESCE):

private static async Task RecomputeTtmCalculationsAsync(SqlConnection con, SqlTransaction? tx, string cust, CancellationToken ct)
{
    if (con == null || string.IsNullOrWhiteSpace(cust)) return;

    var srcCols = new HashSet<string>(StringComparer.OrdinalIgnoreCase);
    bool hasTarget = false;
    try
    {
        using (var c = con.CreateCommand())
        {
            c.Transaction = tx;
            c.CommandText = @"SELECT COLUMN_NAME FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = 'tblMain'";
            using var r = await c.ExecuteReaderAsync(ct).ConfigureAwait(false);
            while (await r.ReadAsync(ct).ConfigureAwait(false))
            {
                var n = Convert.ToString(r.GetValue(0));
                if (!string.IsNullOrWhiteSpace(n)) srcCols.Add(n.Trim());
            }
        }
        using (var t = con.CreateCommand())
        {
            t.Transaction = tx;
            t.CommandText = @"SELECT 1 FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_NAME = 'tblMainTTMCalculations'";
            var probe = await t.ExecuteScalarAsync(ct).ConfigureAwait(false);
            hasTarget = probe != null;
        }
    }
    catch { /* best-effort probes */ }

    string[] required = new[]
    {
        "strCustomerName", "strMonthKey",
        "curInterestExpense", "curProfitBeforeTaxes", "curDepreciation", "curAmortization",
        "curDistributions", "curCPLTD", "curNetChargeOff", "curPrincipalNR", "curAverageGrossNR"
    };
    if (!hasTarget || required.Any(n => !srcCols.Contains(n))) return;

    using var cmd = con.CreateCommand();
    cmd.Transaction = tx;
    cmd.CommandText = @"
WITH cte AS (
    SELECT
        LTRIM(RTRIM(strCustomerName)) AS strCustomerName,
        LTRIM(RTRIM(strMonthKey)) AS strMonthKey,
        SUM(curInterestExpense)   OVER (PARTITION BY LTRIM(RTRIM(strCustomerName)) ORDER BY LTRIM(RTRIM(strMonthKey)) ROWS BETWEEN 11 PRECEDING AND CURRENT ROW) AS curInterestExpenseTTM,
        SUM(curProfitBeforeTaxes) OVER (PARTITION BY LTRIM(RTRIM(strCustomerName)) ORDER BY LTRIM(RTRIM(strMonthKey)) ROWS BETWEEN 11 PRECEDING AND CURRENT ROW) AS curProfitBeforeTaxesTTM,
        SUM(curDepreciation)      OVER (PARTITION BY LTRIM(RTRIM(strCustomerName)) ORDER BY LTRIM(RTRIM(strMonthKey)) ROWS BETWEEN 11 PRECEDING AND CURRENT ROW) AS curDepreciationTTM,
        SUM(curAmortization)      OVER (PARTITION BY LTRIM(RTRIM(strCustomerName)) ORDER BY LTRIM(RTRIM(strMonthKey)) ROWS BETWEEN 11 PRECEDING AND CURRENT ROW) AS curAmortizationTTM,
        SUM(curDistributions)     OVER (PARTITION BY LTRIM(RTRIM(strCustomerName)) ORDER BY LTRIM(RTRIM(strMonthKey)) ROWS BETWEEN 11 PRECEDING AND CURRENT ROW) AS curDistributionsTTM,
        SUM(curCPLTD)             OVER (PARTITION BY LTRIM(RTRIM(strCustomerName)) ORDER BY LTRIM(RTRIM(strMonthKey)) ROWS BETWEEN 11 PRECEDING AND CURRENT ROW) AS curCPLTDTTM,
        SUM(curNetChargeOff)      OVER (PARTITION BY LTRIM(RTRIM(strCustomerName)) ORDER BY LTRIM(RTRIM(strMonthKey)) ROWS BETWEEN 11 PRECEDING AND CURRENT ROW) AS curNetChargeOffTTM,
        AVG(curPrincipalNR)       OVER (PARTITION BY LTRIM(RTRIM(strCustomerName)) ORDER BY LTRIM(RTRIM(strMonthKey)) ROWS BETWEEN 11 PRECEDING AND CURRENT ROW) AS curAveragePrincipalNRTTM,
        AVG(curAverageGrossNR)    OVER (PARTITION BY LTRIM(RTRIM(strCustomerName)) ORDER BY LTRIM(RTRIM(strMonthKey)) ROWS BETWEEN 11 PRECEDING AND CURRENT ROW) AS curAverageGrossNRTTM
    FROM dbo.tblMain
    WHERE LTRIM(RTRIM(strCustomerName)) = LTRIM(RTRIM(@cust))
)
MERGE dbo.tblMainTTMCalculations AS t
USING cte AS s
ON LTRIM(RTRIM(t.strCustomerName)) = s.strCustomerName
AND LTRIM(RTRIM(t.strMonthKey)) = s.strMonthKey
WHEN MATCHED THEN
    UPDATE SET
        t.curInterestExpenseTTM = s.curInterestExpenseTTM,
        t.curProfitBeforeTaxesTTM = s.curProfitBeforeTaxesTTM,
        t.curDepreciationTTM = s.curDepreciationTTM,
        t.curAmortizationTTM = s.curAmortizationTTM,
        t.curDistributionsTTM = s.curDistributionsTTM,
        t.curCPLTDTTM = s.curCPLTDTTM,
        t.curNetChargeOffTTM = s.curNetChargeOffTTM,
        t.curAveragePrincipalNRTTM = s.curAveragePrincipalNRTTM,
        t.curAverageGrossNRTTM = s.curAverageGrossNRTTM,
        t.datCalculationRun = SYSDATETIME()
WHEN NOT MATCHED THEN
    INSERT (strCustomerName, strMonthKey,
            curInterestExpenseTTM, curProfitBeforeTaxesTTM, curDepreciationTTM,
            curAmortizationTTM, curDistributionsTTM, curCPLTDTTM,
            curNetChargeOffTTM, curAveragePrincipalNRTTM, curAverageGrossNRTTM,
            datCalculationRun)
    VALUES (s.strCustomerName, s.strMonthKey,
            s.curInterestExpenseTTM, s.curProfitBeforeTaxesTTM, s.curDepreciationTTM,
            s.curAmortizationTTM, s.curDistributionsTTM, s.curCPLTDTTM,
            s.curNetChargeOffTTM, s.curAveragePrincipalNRTTM, s.curAverageGrossNRTTM,
            SYSDATETIME());";
    AddParam(cmd, "@cust", SqlDbType.NVarChar, cust.Trim());
    try { await cmd.ExecuteNonQueryAsync(ct).ConfigureAwait(false); }
    catch { /* fail-open on persist */ }
}

STEP 2 — ADD this call immediately AFTER the existing RecomputePbtTtmAsync call block (do not modify that block), using in-scope con, tx, cust, ct:

// Recompute all trailing-12-month TTM components (year-agnostic) into tblMainTTMCalculations after persist
try
{
    await RecomputeTtmCalculationsAsync(con, tx, cust, ct).ConfigureAwait(false);
}
catch { /* fail-open on TTM recompute */ }

VERIFY BEFORE SHOWING DIFF (report; do not force):
a) The method compiles in context: AddParam, SqlDbType, con.CreateCommand, ExecuteReaderAsync/ExecuteNonQueryAsync are all already used in this file (confirm). If MERGE/System.Linq Any() needs a using that's missing, report it instead of adding random usings — tell me which using is needed.
b) Confirm the MERGE statement ends with a semicolon (required by SQL Server MERGE).
c) Confirm no existing method/line changed; RecomputePbtTtmAsync untouched.

Show the unified diff. Apply nothing until I confirm.
