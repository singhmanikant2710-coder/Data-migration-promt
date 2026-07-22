Read-only. No edits. No plan. Just report with file paths + exact code. Do NOT modify anyone's work (including Jothi's).

Context (UAT #141): On the Review Queue page, add a Sample Name dropdown filter. Currently there is only a free-text search bar which can't do combination filters (e.g. Sample AND Reviewer). The dropdown should let the user filter the grid to a selected sample, and the existing search should further narrow within that (AND behaviour).

A very similar Sample Name dropdown already exists on the Review Status page (UAT #77) — it lists open samples and filters the grid.

Report:
1) The Review Queue page component (frontend/src/app/review-queue/page.tsx). Paste: the grid data source, how rows are currently filtered by the search box, and the existing filter controls (My View dropdown, search box, page size). What field on each row holds the sample name/id?
2) Does the Review Queue row data already include the sample name / sample id per row? Show the row type and where it comes from (API service + backend).
3) On the Review Status page (frontend/src/app/review-status/page.tsx), show how its Sample Name dropdown is built: where it gets the options (the samples lookup API), and how selecting a sample filters the grid. This is the pattern to reuse.
4) Is there an existing samples lookup the Review Queue can reuse to populate the dropdown (e.g. the one Review Status uses)? Show it.
5) State exactly what must change and in how many files to add a Sample Name dropdown to Review Queue that filters the grid by the selected sample, combined (AND) with the existing search box. Prefer client-side filtering if the row data already contains the sample name; note if a backend change is needed.

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
