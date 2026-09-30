Context: CASRR. Relationship_mgr_number, Portfolio_mgr_number (02_CORE_02_Reviews)
and Recipient_role (Distribution Parties) are now nvarchar(10), zero-padded to
5 chars by a DB trigger. The API still reads them via Convert.ToInt32 and
int-typed properties, which would throw on any non-numeric value.

Change these RM/PM number / Employee ID properties to string end-to-end
(DTOs, response contracts, SqlParameter bindings as NVarChar(10), frontend
types). The fix must be generic and safe:
- Matching/joins must keep working: keep TRY_CONVERT(int, ...) comparisons in
  SQL as-is, and do not introduce any C# int parsing that can throw.
- Display must stay identical, e.g. "17436 - WAGNER, JOHN C". Zero-padded IDs
  show exactly as stored.
- NULL / empty must stay empty. Never show "0", "00000", or "00000 - NAME".
- No change to sample-load NULL-on-miss or the canonical-name save behaviour.
- The SplitNumberName / dropdown-value parsing must keep working with both
  padded ("00030") and unpadded ("30") values.

Report:
- ADDED and REMOVED lines (file:line)
- On-screen behaviour change, if any, for: Customer Info RM/PM, Distribution
  Parties screen, and reports showing these IDs
- The NULL-value case for each of these screens
- Build results for backend and frontend
Do not commit until I review.
