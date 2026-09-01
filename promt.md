SINGLE-FILE, BOUNDED EDIT. Only edit backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs, only the previous-month fiscal override block we just added in UpsertRowWithConnectionAsync. Show unified diff BEFORE applying.

BUG: Our new code reads prev fiscal values with prdr.GetInt32(0)/GetInt32(1), but intFiscalYear/intFiscalMonth columns are SMALLINT (Int16) in SQL Server. GetInt32 throws "Unable to cast object of type 'System.Int16' to type 'System.Int32'." when adding a new month.

FIX: Replace GetInt32 with a type-safe conversion using Convert.ToInt32 on the raw value, which handles Int16/Int32/etc.

Current:
    using var prdr = await prevCmd.ExecuteReaderAsync(ct).ConfigureAwait(false);
    if (await prdr.ReadAsync(ct).ConfigureAwait(false))
    {
        if (!prdr.IsDBNull(0)) prevFy = prdr.GetInt32(0);
        if (!prdr.IsDBNull(1)) prevFm = prdr.GetInt32(1);
    }

Change to:
    using var prdr = await prevCmd.ExecuteReaderAsync(ct).ConfigureAwait(false);
    if (await prdr.ReadAsync(ct).ConfigureAwait(false))
    {
        if (!prdr.IsDBNull(0)) prevFy = Convert.ToInt32(prdr.GetValue(0));
        if (!prdr.IsDBNull(1)) prevFm = Convert.ToInt32(prdr.GetValue(1));
    }

Only this change (GetInt32 → Convert.ToInt32(prdr.GetValue(...))). Nothing else. Convert.ToInt32 safely handles Int16 (smallint), Int32, and other numeric types.

VERIFY BEFORE SHOWING DIFF:
a) Only the two GetInt32 lines changed to Convert.ToInt32(prdr.GetValue(...)).
b) Rest of the override block unchanged.

Show the unified diff. Apply nothing until I confirm.
