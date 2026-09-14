Bug 227 fix — Geoff confirmed: keep full timestamp for Sample.Created_date, use ISO date format (yyyy-MM-dd, e.g. 2026-03-04) for all other date fields across the 4 in-scope CORE exports. SINGLE FILE. Show diff, do NOT commit.

FILE: backend/src/Casrr.Api/Controllers/ExportsController.cs

Modify the call site (line ~118, inside WriteCsvFromSqlAsync) where ToInvariantString is called per-cell — pass the column name (already available via the reader in that loop) into the formatting logic:
- If column name is "Created_date" (case-insensitive, specific to 02_CORE_01_Samples): keep dt.ToString("yyyy-MM-ddTHH:mm:ss")
- For all other DateTime columns: dt.ToString("yyyy-MM-dd", CultureInfo.InvariantCulture)

Keep DateTimeOffset and the IFormattable fallback unchanged.

Show diff. Rebuild. Do NOT commit.
