Backend only. Files: SqlInitialMemoReportRepository.cs and SqlFinalMemoReportRepository.cs.
Do not plan. Just apply.

The CustomerBackground read uses a hardcoded ordinal 26, which is fragile and may be wrong. Replace both hardcoded ordinal reads with a name-based lookup instead:

  var ordCb = rdr.GetOrdinal("Borrower_information");
  CustomerBackground = rdr.IsDBNull(ordCb) ? null : Convert.ToString(rdr.GetValue(ordCb), us)

Do not change any other column read, and do not change the SELECT statements.
