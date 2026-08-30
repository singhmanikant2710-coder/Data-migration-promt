In backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs, method GetCurrentYearSeriesAsync. It currently filters by calendar year:
    WHERE ... AND LEFT(LTRIM(RTRIM(strMonthKey)),4) = @yrStr

This returns calendar-year months (202501-202512) instead of fiscal-year months. For BHG (Oct fiscal year), fiscal 2025 = 202510-202609, so 202601-202605 are missing from the series, and selecting them shows no match (falls back to 202512).

Change it to be fiscal-year-aware, mirroring GetHistoricYearSeriesAsync. Replace the calendar-only query with a schema-probe + fiscal-year filter:

    bool hasFiscalYear = false;
    try
    {
        using var colCmd = con.CreateCommand();
        colCmd.CommandText = @"SELECT 1 FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = 'tblMain' AND COLUMN_NAME = 'intFiscalYear'";
        var probe = await colCmd.ExecuteScalarAsync(ct).ConfigureAwait(false);
        hasFiscalYear = probe != null;
    }
    catch { }

    using var cmd = con.CreateCommand();
    if (hasFiscalYear)
    {
        cmd.CommandText = @"
SELECT *
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = LTRIM(RTRIM(@cust)) AND intFiscalYear = @yr
ORDER BY LTRIM(RTRIM(strMonthKey)) ASC";
        AddParam(cmd, "@cust", SqlDbType.NVarChar, custResolved);
        AddParam(cmd, "@yr", SqlDbType.Int, fiscalYear);
    }
    else
    {
        cmd.CommandText = @"
SELECT *
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = LTRIM(RTRIM(@cust))
    AND LEFT(LTRIM(RTRIM(strMonthKey)),4) = @yrStr
ORDER BY LTRIM(RTRIM(strMonthKey)) ASC";
        AddParam(cmd, "@cust", SqlDbType.NVarChar, custResolved);
        AddParam(cmd, "@yrStr", SqlDbType.NVarChar, fiscalYear.ToString("0000", CultureInfo.InvariantCulture));
    }

Ensure exactly ONE "using var cmd" (remove any old one and old AddParam). Keep the reader loop and everything after unchanged. Show me the method from signature to the reader loop before applying.
