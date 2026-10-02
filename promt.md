CRM Findings and Observations PDF: three formatting changes to mirror the
"CRM Findings for Management" PDF (screenshot attached):
1. Page header: make the header bar height and the date font (size/weight)
   identical to CRM Findings for Management.
2. Component titles: remove the "##-" prefix, e.g. "01-RISK RECOGNITION" →
   "RISK RECOGNITION", "02-SCORECARD MANAGEMENT" → "SCORECARD MANAGEMENT".
   Strip only a leading "digits + hyphen" pattern; don't touch any other
   text, and leave sort order unchanged.
3. Table header row (CUSTOMER NAME (REVIEW ID) / SEVERITY / COMMENTS): use
   the same colour scheme as CRM Findings for Management (dark navy
   background, white text).

Constraints: change only this report. If the header/table styles come from
a shared component (e.g. pageSetup.ts), reuse the style the Management
report already uses; don't change shared defaults that other reports
depend on. Don't change data, filters, grouping, columns, or page breaks.

Report ADDED/REMOVED (file:line), the on-screen change for each of the 3
items, a component title that has no number prefix (unchanged), and NULL
cases (blank comments/severity render as before). Confirm no other report
file changed. Build result. Do not commit.

