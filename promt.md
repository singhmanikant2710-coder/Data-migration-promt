READ-ONLY. After restart + cache clear, both /api/v1/metrics/current-year
and /api/v1/metrics/rolling24 return MinTangibleNetWorth = 62297 for
ATHENS PAPER 202605. In the DB, for 202605 both
tblMainCovenants.strCovenantActual and tblMain.dblCovenantActual1 are
NULL. Only 202603 has 62297.

Trace exactly where 62297 for 202605 comes from: both covenant merge
blocks in SqlMainRepository (~1433-1508, ~1977-2039),
TryMergeCovenantsIntoSeries, and any "latest up to month",
previous-month, carry-forward or tblCustomer fallback on the backend.
Quote file:line of the line that supplies the value, and list every
other place with the same pattern. Report only.
