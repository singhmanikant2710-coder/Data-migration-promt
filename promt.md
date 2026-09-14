Bug 227 — CORE Exports date fields should be Date Only (MM-DD-YYYY) for 02_CORE_01_Samples, 02_CORE_02_Reviews, 02_CORE_04_Accounts, 02_CORE_05_Covenants. READ-ONLY, no edits. One pass, answer, STOP.

Investigate:
1. Find ExportsController.cs (or wherever these 4 exports are generated — CSV via WriteCsvFromSqlAsync, same file as Bug 222). Find ToInvariantString (or whatever formats cell values before writing to CSV).
2. How are DateTime/DateOnly values currently formatted? Paste the exact format string used today (we saw 'yyyy-MM-ddTHH:mm:ss' during Bug 222 investigation — confirm if that's still the case, or if it changed).
3. For EACH of the 4 exports (Samples, Reviews, Accounts, Covenants), confirm the underlying SQL column types for their date columns — are they DATETIME2, DATE, or something else in the DB? This matters because a DATETIME2 column with a time component of 00:00:00 vs one with a real time value need to be handled the same way (format-only, not data truncation).
4. Is the formatting logic SHARED across all exports (one ToInvariantString used everywhere), or does each export have its own formatting path? This determines whether one change covers all 4, or 4 separate changes are needed.
5. Are there any OTHER exports (besides these 4) that also go through the same formatting function? If so, would fixing it there affect exports NOT in scope for this bug (e.g. the other 3 CORE exports, or non-CORE reports)?

Report the exact format string, whether it's shared, and the blast radius of a change. Do NOT propose or write a fix yet.
