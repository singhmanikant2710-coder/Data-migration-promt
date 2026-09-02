Bug 196 — READ-ONLY, no edits. One pass. Open, answer, STOP.

Open backend/src/Casrr.Api/Controllers/SelectionsController.cs — the full PUT library/{id} method (~162-195).
Open backend/src/Casrr.Infrastructure/Repositories/SelectionRepository.cs — the full UpdateAsync method.

Answer:
1. Does UpdateAsync return affected-row count, and does the controller (or repo) do anything when 0 rows are affected — e.g. call AddAsync/InsertAsync as a fallback? Paste those exact lines.
2. Paste the exact @section value binding — is it coming from the query string 'section' param, and is it trimmed/normalized before binding?
3. In the frontend save handler (maintenance/selections/page.tsx ~line 433), what exact 'section' string is sent — row.original.section as-is, or a label? Paste that line.

Then STOP.
