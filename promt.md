Now APPLY the change we reviewed. Only edit backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs. Add-only — do not modify or remove any existing method or line.

Apply exactly these two additions (the version we finalized, with the corrections — no duplicate columns, no space in aliases, curCPLTD consistent):

1) ADD this new method immediately AFTER the existing RecomputePbtTtmAsync method:

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

2) ADD this call immediately AFTER the existing "await RecomputePbtTtmAsync(con, tx, cust, fy, ct)" try/catch block (do not modify that block), using in-scope con, tx, cust, ct:

// Recompute all trailing-12-month TTM components (year-agnostic) into tblMainTTMCalculations after persist
try
{
    await RecomputeTtmCalculationsAsync(con, tx, cust, ct).ConfigureAwait(false);
}
catch { /* fail-open on TTM recompute */ }

Apply now. After applying, confirm: the method was added after RecomputePbtTtmAsync, the call was added after the RecomputePbtTtmAsync call, and no existing code was changed. Do not run build. Then STOP.
