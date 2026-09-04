Bug 195 — Reports > Sample Name Selection dropdown sorts by Sample Start Date but should sort by Sample ID descending. READ-ONLY, no edits. One pass, answer, STOP.

The "Sample Name Selection" dropdown on the Reports screen lists samples like "362 - 10/1/2026 - Examination - RB - Central". Geoff reports the options currently descend by Sample Start Date; they should descend by Sample ID (e.g. 374, 370, 369, 366, 362, 361, 360...).

Find where this dropdown's options are sourced and sorted:
1. Frontend Reports page — find the Sample Name Selection dropdown and how it loads/orders its options. Is there a client-side sort? What key does it sort by (sample date vs sample id)? Paste that code + file/line.
2. Backend — find the endpoint/query that returns the sample list for this dropdown (search: samples list, ReportsController, sample selection, ORDER BY in the samples query). Paste the exact ORDER BY. Is it ordering by Sample_start_date / Sample_date, or by Sample_id?
3. Identify the single place where the sort order is decided (frontend sort OR backend ORDER BY) that needs to change to "Sample_id DESC".

Report file paths + line numbers + the current sort key. Do NOT fix yet.
