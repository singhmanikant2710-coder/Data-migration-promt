SINGLE-FILE, BOUNDED EDIT. Only edit backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs, only GetCurrentYearSeriesAsync. Show unified diff BEFORE applying. Do not run build.

BUG: GetCurrentYearSeriesAsync filters by CALENDAR year (LEFT(strMonthKey,4) = @yrStr), but the month-options endpoint (GetMonthKeySeriesForYearAsync) and the Year dropdown use FISCAL year (intFiscalYear). For BHG (October fiscal year), fiscal year 2025 contains calendar months 202510-202606. The calendar filter returns only 202510-202512, so selecting 202601+ shows a blank UI (no row). The fix: make GetCurrentYearSeriesAsync fiscal-year aware, exactly like GetHistoricYearSeriesAsync already does (schema-probe intFiscalYear, use it if present, else fall back to calendar).

FIX: In GetCurrentYearSeriesAsync, replace the single calendar-year query with the same fiscal-aware pattern used in GetHistoricYearSeriesAsync:

CURRENT (calendar-only):
    using var cmd = con.CreateCommand();
    // Enforce calendar-year filtering and ordering by monthKey for Blackbook parity
    cmd.CommandText = @"
SELECT *
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = LTRIM(RTRIM(@cust))
    AND LEFT(LTRIM(RTRIM(strMonthKey)),4) = @yrStr
ORDER BY LTRIM(RTRIM(strMonthKey)) ASC";
    AddParam(cmd, "@cust", SqlDbType.NVarChar, custResolved);
    AddParam(cmd, "@yrStr", SqlDbType.NVarChar, fiscalYear.ToString("0000", CultureInfo.InvariantCulture));

REPLACE WITH (fiscal-aware, mirroring GetHistoricYearSeriesAsync):
    // Prefer fiscal-year filtering when the intFiscalYear column exists (needed for non-December fiscal years
    // like BHG where fiscal year 2025 includes calendar months 202601-202606). Fall back to calendar year otherwise.
    bool hasFiscalYear = false;
    try
    {
        using var colCmd = con.CreateCommand();
        colCmd.CommandText = @"SELECT 1 FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = 'tblMain' AND COLUMN_NAME = 'intFiscalYear'";
        var probe = await colCmd.ExecuteScalarAsync(ct).ConfigureAwait(false);
        hasFiscalYear = probe != null;
    }
    catch
    {
        // best effort
    }

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

Keep everything AFTER this (the reader loop, mapping, telemetry, return) exactly the same. Only the query-building portion changes.

VERIFY BEFORE SHOWING DIFF:
a) GetCurrentYearSeriesAsync now probes intFiscalYear and uses intFiscalYear = @yr when present, else calendar fallback.
b) The pattern matches GetHistoricYearSeriesAsync exactly (same probe, same @yr int binding).
c) The reader loop / mapping / return after the query are unchanged.
d) No other method changed.

Show the unified diff. Apply nothing until I confirm.
