Bug 222 — CORE Exports data quality issue. READ-ONLY, no edits. One pass, answer, STOP.

Screen: Maintenance > CORE Exports, exporting table 02_CORE_02_Reviews to Excel (.xlsx).
Two issues reported by Geoff:
1. Some exported rows contain data inconsistent with the table format (rows 6-10, 17-18, 20-25) — possibly bogus/duplicate records.
2. Columns B, BO, BP, BT, BW, CD, CE, CG, CI, CK, CL, CM contain "formatting jargon" (likely raw HTML tags, rich-text markup, or encoding artifacts) that should be cleaned.

Trace the CORE Export logic for 02_CORE_02_Reviews:
1. Find the export endpoint + service/repository that generates the 02_CORE_02_Reviews export. Search: ExportsController, "CORE", "export", "02_CORE_02_Reviews", xlsx/Excel generation. Report the file(s) + method(s) and how the Excel is built (library used, e.g. ClosedXML/EPPlus/SheetJS).
2. Report the exact SQL/query that pulls the rows for this export. Does it JOIN or UNION anything that could introduce duplicate/bogus rows (issue 1)? Is there any filter missing (e.g. cancelled/soft-deleted rows included)?
3. For issue 2 — map the reported columns to source fields. Which DB columns feed export columns B, BO, BP, BT, BW, CD, CE, CG, CI, CK, CL, CM? Are any of these rich-text/HTML fields (e.g. guidance, comments, description, justification) that are written to the cell WITHOUT stripping HTML tags/entities? Report which fields carry HTML/markup and whether any sanitization/strip is applied before writing to the cell.
4. Is there existing HTML-stripping/sanitization logic elsewhere in the codebase (e.g. used in PDF or UI rendering) that could be reused for the export?

Report file paths + line numbers + the query + which columns carry raw HTML. Do NOT propose or write a fix yet.
