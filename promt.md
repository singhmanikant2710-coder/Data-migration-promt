Single-file edit: frontend/src/components/pdf/CrmSummaryTablePDF.tsx

Fix: All report footers should read "[Report Name] • Page # of ##". Currently the footer includes the caption (Sample Name): "CRM Summary Table • ${caption} • Page X of Y". Remove the caption segment and update the report name to "CRM Findings Summary Table". This footer text appears in TWO places (the main page footer and the final filter-payload page footer) — update BOTH occurrences identically.

BEFORE (both occurrences):
render={({ pageNumber, totalPages }) => `CRM Summary Table • ${out(caption)} • Page ${pageNumber} of ${totalPages}`}

AFTER (both occurrences):
render={({ pageNumber, totalPages }) => `CRM Findings Summary Table • Page ${pageNumber} of ${totalPages}`}

CONSTRAINTS:
- ONLY change the footer render string, in both places it appears.
- Remove the `${out(caption)}` segment entirely from the footer.
- Do NOT change the footer styles, border, positioning, or the page-number render logic.
- Do NOT remove or alter the `caption` variable itself (it may still be used by the header) — only stop using it in the footer text.
- Do NOT touch any other file.
- Only edit this one file. Show the diff.
