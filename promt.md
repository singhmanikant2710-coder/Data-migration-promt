Stages 1-2 accepted. The DBA script has been run in Dev; both tables exist
(dbo.[04_TEMP_03_Distribution Parties] and
dbo.[05_AUDIT_01_Distribution Parties Uploads]). Continue with stage 3
(backend), then stage 4 (frontend), following the original requirements
unchanged:

- Whole-file validation before any write, all errors listed per row;
  nothing written on any error: blank/invalid email, duplicate email
  (case-insensitive), blank name, blank/non-numeric/>10-digit ID,
  duplicate ID compared numerically ("30" = "00030").
- Header matching: "Employee ID", "Name", "Email", case-insensitive,
  trimmed, any column order. Accept the DB names (Recipient_role,
  Recipient_name, Recipient_email) as aliases. Ignore extra columns.
  Missing required column → clear message naming it. Template download
  uses the friendly headers.
- Shared EmployeeId rule (lib/employeeId.ts and Casrr.Domain.EmployeeId):
  pad to 5, max 10, numeric duplicate check. Don't redefine it.
- XLSX via lib/xlsxCell.ts (don't write a new helper). Employee ID read as
  TEXT in CSV and XLSX.
- Preview diff vs the current table (key = email): added / removed /
  changed (ID compared numerically) / unchanged count. Show the removed
  list.
- Atomic staging + swap in ONE backend call and ONE transaction, with
  SqlBulkCopyOptions.FireTriggers; on any failure the live table stays
  intact, and NOLOCK readers never see an empty table.
- Audit row: who, when, file name, counts added/removed/changed.
- If either table is missing, fail with the clear "run scripts/sql/...
  first" message. Never create objects at runtime.
- Admin-only page (same admin gate) + [Authorize(Policy="RequireAdmin")].
- No changes to dropdowns, the resolver, sample-load, or reports.

If a stage gets too large, stop at a clean, buildable boundary and tell me
exactly what remains. No half-wired UI.

Report: ADDED/REMOVED (file:line), the on-screen flow, and behaviour for:
valid file, file with errors, duplicate ID "30"/"00030", Excel-numeric ID
cell, 6-digit ID (accepted, stored as-is), 11-digit ID (rejected), missing
header, extra columns, upload failure mid-way (live data intact), empty
file, tables missing (clear message). NULL cases: blank cells in each
column. Build results. Do not commit.
