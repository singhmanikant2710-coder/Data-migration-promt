SINGLE-FILE, BOUNDED EDIT. Only edit backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs, method UpsertRowWithConnectionAsync. Show unified diff BEFORE applying. Do not run build.

BUG: When a NEW month row is inserted (Add New Month), intFiscalYear/intFiscalMonth are derived from the CALENDAR (DeriveFiscalFromMonthKey returns year=yyyy, month=mm). This is only correct for January-fiscal-year customers. For non-January customers (e.g. BHG=October, Van Zyverden=July, World Acceptance=April), fiscal conventions differ, so calendar-derived values are wrong (e.g. BHG 202606 stored as FY2026/6 instead of the correct continuation of the fiscal sequence).

FIX: For the INSERT case (new month, !exists) ONLY, derive the new row's fiscal from the PREVIOUS existing month + 1, which is convention-independent and matches existing data for ALL customers:
   new fiscalMonth = (prev.intFiscalMonth == 12) ? 1 : prev.intFiscalMonth + 1
   new fiscalYear  = (prev.intFiscalMonth == 12) ? prev.intFiscalYear + 1 : prev.intFiscalYear
If there is NO previous month (first ever month for the customer), fall back to the existing calendar-derived (fy, fm) as-is.

In UpsertRowWithConnectionAsync, AFTER the `exists` check is known and BEFORE the `if (exists) { ... } else { ... }` insert/update split, add logic that — only when NOT exists — queries the previous month and overrides fy/fm:

Add a query using the SAME pattern already used elsewhere in this file (SELECT TOP (1) ... WHERE strMonthKey < @mk ORDER BY strMonthKey DESC), reading the previous row's intFiscalYear and intFiscalMonth:

    // For a NEW month, derive fiscal from the previous existing month + 1
    // (convention-independent; works for all fiscal-year-start months).
    if (!exists)
    {
        int? prevFy = null;
        int? prevFm = null;
        using (var prevCmd = con.CreateCommand())
        {
            prevCmd.Transaction = tx;
            prevCmd.CommandText = @"
SELECT TOP (1) intFiscalYear, intFiscalMonth
FROM dbo.tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = LTRIM(RTRIM(@cust))
  AND LTRIM(RTRIM(strMonthKey)) < LTRIM(RTRIM(@mk))
ORDER BY LTRIM(RTRIM(strMonthKey)) DESC";
            AddParam(prevCmd, "@cust", SqlDbType.NVarChar, cust);
            AddParam(prevCmd, "@mk", SqlDbType.NVarChar, mk);
            using var prdr = await prevCmd.ExecuteReaderAsync(ct).ConfigureAwait(false);
            if (await prdr.ReadAsync(ct).ConfigureAwait(false))
            {
                if (!prdr.IsDBNull(0)) prevFy = prdr.GetInt32(0);
                if (!prdr.IsDBNull(1)) prevFm = prdr.GetInt32(1);
            }
        }
        if (prevFy.HasValue && prevFm.HasValue && prevFm.Value >= 1 && prevFm.Value <= 12)
        {
            fm = (prevFm.Value == 12) ? 1 : prevFm.Value + 1;
            fy = (prevFm.Value == 12) ? prevFy.Value + 1 : prevFy.Value;
        }
        // else: no valid previous month -> keep calendar-derived (fy, fm) as fallback
    }

Ensure this override runs BEFORE fy/fm are used in the INSERT column values (the `orderedValues` list uses fy, fm). The existing `var (fy, fm) = DeriveFiscalFromMonthKey(mk);` must be a MUTABLE local (change to `var` that can be reassigned — if fy/fm are from a tuple deconstruction they're already reassignable locals; confirm).

STRICT — do NOT change:
- The UPDATE (exists==true) path — leave it as-is (it keeps calendar-derived, which is fine for existing rows and not the bug).
- DeriveFiscalFromMonthKey itself.
- Any other method.

VERIFY BEFORE SHOWING DIFF:
a) The previous-month override runs ONLY when !exists.
b) It reads prev intFiscalYear/intFiscalMonth via the SELECT TOP(1) ... strMonthKey < @mk pattern.
c) fm/fy are reassigned before the INSERT orderedValues use them.
d) Fallback to calendar-derived when no previous month.
e) fy/fm are reassignable locals (not readonly).

Show the unified diff. Apply nothing until I confirm.
