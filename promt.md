Stage 3 accepted. Continue with stage 4 (frontend), as you listed:
1. services/api/distributionPartiesUpload.ts: typed client for
   POST /api/v1/distribution-parties-upload/process.
2. Template download with the friendly headers "Employee ID, Name, Email"
   (CSV).
3. app/admin/distribution-parties-upload/page.tsx: template download →
   choose file (.csv and .xlsx) → parse (CSV via the same PapaParse setup
   as the monthly upload; XLSX via lib/xlsxCell.ts, Employee ID as text) →
   header mapping (case-insensitive, trimmed, any order, DB-name aliases,
   extra columns ignored, missing column → message naming it) → POST
   preview → show either the error table (row, field, value, message) or
   the diff (added / removed / changed counts + the removed list +
   unchanged count) → Confirm → POST commit → success summary with the
   counts.
4. Add a link to the page wherever the other admin pages are listed
   (same pattern as Monthly Upload).
5. Confirm is disabled while there are errors or while a request is in
   flight. Show a clear warning that confirming replaces the whole table.
6. Don't change any other page or the backend.

Report ADDED/REMOVED (file:line), the on-screen flow, and behaviour for:
valid file, file with errors, duplicate ID "30"/"00030", Excel-numeric ID
cell (incl. a "00030"-formatted number), 6-digit ID, 11-digit ID, missing
header, extra columns, DB-name headers, an empty file, and tables missing
(the 400 message shown). NULL cases: blank cells in each column. Build
results. Do not commit.
