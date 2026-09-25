READ-ONLY. /api/v1/metrics/current-year (noCache=true) returns
MinTangibleNetWorth = 62297 for ATHENS PAPER 202605, but in the DB both
tblMainCovenants.strCovenantActual and tblMain.dblCovenantActual1 are
NULL for 202605 (only 202603 has 62297).

Trace exactly how that value is produced: GetCurrentYearSeriesAsync,
both covenant merge blocks (~1433-1508, ~1977-2039),
TryMergeCovenantsIntoSeries, and any "latest up to month" / previous
month / tblCustomer fallback. Quote file:line of the line that
supplies 62297 for 202605. Report only.
