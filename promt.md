Fix — prevent Excel from misreading "N-NN" style text values (Delinquent_status: "1-30", "30-59", etc.) as dates in the 02_CORE_04_Accounts CSV export. SINGLE FILE, scoped to ONE column. Show diff, do NOT commit.

FILE: backend/src/Casrr.Api/Controllers/ExportsController.cs

In the per-cell formatting/writing logic (inside WriteCsvFromSqlAsync, where column names are available via the reader), add a check: if the column name is "Delinquent_status" (case-insensitive, specific to 02_CORE_04_Accounts) AND the value is a non-empty string, wrap it so Excel treats it as literal text instead of auto-converting it to a date on open. Use the ="value" Excel formula-text trick (e.g. if the value is "1-30", write it as ="1-30" in the CSV cell) — this is the most reliable cross-version method.

Handle CSV-escaping correctly: the ="..." wrapper must still go through EscapeCsv properly.

Apply this ONLY to the Delinquent_status column — do NOT apply broadly to all string cells or other columns.

Show diff. Rebuild. Do NOT commit.
