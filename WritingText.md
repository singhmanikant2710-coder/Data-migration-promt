UAT #258 – Sample upload / Load Samples only loads the first 25 sample selections

ISSUE (Geoff)
The Sample loading workflow loads only the first 25 selections from an uploaded CSV/Excel file. Files contain 1–120 selections depending on the project (most ≤ 60). Business would accept a limit of 75 (and split larger files), but prefers fewer manual splits.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- Trace the full flow with file:line: Samples / Load Samples screen → file parse (CSV and XLSX, lib/xlsxCell.ts) → preview/grid → Add Customer / Checklist → Validate → Load (creates Reviews) → backend controller/service/repository (SqlSampleLoadRepository).
- Find EXACTLY where 25 comes from (hardcoded constant, page size, slice, TOP 25, API paging, request size, default parameter). Is it a hard cut (rows 26+ dropped silently) or just display paging (rows exist but are not loaded)? Report every place a limit applies, frontend and backend.
- Per-row cost of Load: queries/inserts per selection (Reviews, Accounts, checklists), transaction scope, command timeout, HTTP/request timeout (Next.js route, Kestrel, App Service ~230 s), payload size limits. Estimate time for 75 and 120 rows.
- Duplicate protection from UAT #229 (no duplicate reviews per sample/customer) – must stay intact.
- Is the user told today that rows beyond 25 were ignored?

PHASE 2 – PLAN (smallest generic change)
- One named constant (e.g. MAX_SAMPLE_SELECTIONS) shared by frontend and backend; no other hardcoded 25.
- Rows above the limit must never be dropped silently: show a clear message before Load ("This file has N selections; the maximum per upload is X. Please split the file.") and block, OR load all if within the limit.
- Keep #229 duplicate checks, validation, and existing behaviour for files ≤ 25 rows exactly as today.
- Do NOT decide silently – list options with a recommendation:
  1. Limit value: 75 (Geoff's minimum) or 120 (covers all known files) – based on the measured/estimated Load time and timeouts.
  2. Over the limit: block with message, or load the first X and list the rest?
  3. Does the grid need paging/scroll changes to show up to the limit?
- No schema change. SQL only as idempotent scripts/sql/ if truly needed – ask first.

PHASE 3 – IMPLEMENT (after my answers)

PHASE 4 – VERIFY + REPORT (in chat, not in a file)
- Root cause (file:line), ADDED / CHANGED / REMOVED (file:line).
- Before/after: files with 10, 25, 26, 60, 75 (and 120 if chosen) selections, CSV and XLSX; file over the limit; duplicates within the file; customer already loaded in the sample (#229).
- Load time measured or estimated for the largest case; confirm no timeout.
- Read-only SQL to confirm the number of Reviews created per Sample_id matches the file.
- dotnet build + tests, npx tsc --noEmit, npm run build.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo; leave pre-existing SQL scripts and devin-output/ alone).
- Do not commit or push.
