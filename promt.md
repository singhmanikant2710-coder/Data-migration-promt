Read-only. No edits. No plan. Just report with file paths + code.

Issue: In the Customer Background / Customer Info Comments rich-text field, users insert tables (built or pasted from Excel). The table HTML saves fine and renders in the web UI, but in the generated CAS Linesheet PDF the table collapses into a single wrapped line of text instead of rendering as a table.

Report:
1) Which component/service generates the CAS Linesheet PDF? Server-side (e.g. a .NET PDF library, headless browser, ReportLab, wkhtmltopdf) or client-side? Give the file path and library.
2) How does the PDF pipeline consume the comments field — does it receive raw HTML, plain text, or a stripped/sanitised version? Show the exact code that reads the comments HTML and passes it into the PDF.
3) Is there any HTML sanitisation / tag-stripping step that removes <table>/<tr>/<td> before rendering? Show it.
4) Does the PDF renderer support HTML tables at all with the current setup?

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
