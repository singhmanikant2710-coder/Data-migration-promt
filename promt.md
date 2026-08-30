The previous diff was broken: it left the OLD "using var cmd = con.CreateCommand();" and the OLD AddParam lines in place, AND added new ones — causing duplicate 'cmd' declaration and duplicate AddParam (build errors). 

Fix GetCurrentYearSeriesAsync cleanly. REPLACE the entire block starting from the OLD "using var cmd = con.CreateCommand();" line through its AddParam lines (the calendar-year query setup) with this single clean version. There must be exactly ONE "using var cmd", no leftover old query or old AddParam lines.

The correct block (replace the old cmd/query/params setup entirely with this):

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

CRITICAL:
- Delete the OLD "using var cmd = con.CreateCommand();" line (the original one before the query) — do NOT keep two.
- Delete the OLD calendar-year cmd.CommandText and its two AddParam lines — they must NOT remain above this block.
- Result: exactly ONE "using var cmd", exactly one set of AddParam inside each branch, no duplicates.
- Keep everything AFTER (reader loop, mapping, telemetry, return) unchanged.

After applying, show me the COMPLETE GetCurrentYearSeriesAsync method from signature to the first "using (var rdr..." (or reader loop), so I can confirm there is exactly ONE "using var cmd" and no leftover duplicate query/params.
