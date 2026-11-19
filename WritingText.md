1: split Customer Name and Review ID.
2: both summary and detail sheets, as listed.
3: empty cell for NULL numbers/dates; keep the PDF text labels ("Unsatisfactory"/"--", "Yes"/"No").
4: real applied filters for all six. Do not change the Policy Exceptions PDF; I will log that bug separately.
5: stacked 12-month blocks exactly as the PDF.
6: leave the status date out.
7: still download header-only sheets plus Filters, with a toast saying 0 rows.
8: no row cap; truncation marker and 31-char sheet names as proposed.
9: export the pure PDF helpers and reuse them; PDF output must not change (confirm).
10: remove the old Policy Exceptions export branch only after confirming no other report can reach it (file:line).
11: OK – do not change the old guards. Regression must confirm CRM Findings & Observations and Covenants Summary still export their own workbooks.
Proceed with Phase 3 and 4. Report ADDED/CHANGED/REMOVED (file:line), npx tsc --noEmit, npm run build, git status. Do not commit or push.
