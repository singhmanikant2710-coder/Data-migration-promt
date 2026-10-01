Context: CASRR. Build the Distribution Parties upload, per the client:
- An admin uploads the full bank employee file (~15k rows), CSV or XLSX,
  monthly/quarterly.
- Each upload FULLY REPLACES dbo.[03_LIBRARY_10_Distribution Parties]
  (departed associates drop off). Maintenance-screen edits are a temporary
  bridge and are replaced by the next upload.
- Column headers: "Employee ID", "Name", "Email". Match case-insensitively,
  trim whitespace, any column order. Also accept the DB names
  (Recipient_role, Recipient_name, Recipient_email) as aliases. Ignore
  extra columns. If a required column is missing, reject the file with a
  clear message naming it. The template download uses the friendly headers.
- Employee IDs: today they are exactly 5 digits with leading zeros. The
  columns are nvarchar(10) to allow longer IDs in the future.
Use your earlier investigation as the design base.

Requirements:
1. New admin page (same admin gate + [Authorize(Policy="RequireAdmin")])
   with a template download, then parse → validate → preview → confirm.
2. Employee ID rule, defined ONCE as shared constants
   (EMPLOYEE_ID_PAD_WIDTH = 5, EMPLOYEE_ID_MAX_LENGTH = 10):
   digits only, 1 to 10 digits; IDs shorter than 5 are left-padded with
   zeros to 5; IDs of 5-10 digits are stored as-is. Read the ID as TEXT in
   CSV and XLSX; for XLSX reuse lib/xlsxCell.ts (don't write a new helper).
3. Apply the same shared constants to the existing validators so upload
   and screen agree: distribution-parties/page.tsx (EMPLOYEE_ID_MAX,
   maxLength) and DistributionPartiesController.ValidateRole (currently
   5). This is the ONLY change allowed on the maintenance screen.
4. Whole-file validation before any write, all errors listed per row:
   blank/invalid email, duplicate email (case-insensitive), blank name,
   blank/non-numeric/>10-digit ID, duplicate ID compared numerically
   ("30" = "00030"). Any error → nothing written.
5. Preview diff vs the current table (key = email): added / removed /
   changed (ID compared numerically) / unchanged count. Show the removed
   list.
6. Atomic replace in ONE backend call and ONE transaction; on any failure
   the old table stays intact. Prefer staging table + swap so NOLOCK
   readers never see an empty table. Pad IDs in the loader per rule 2 AND
   use SqlBulkCopyOptions.FireTriggers.
7. Audit row: who, when, file name, counts added/removed/changed.
8. Any NEW database object (staging table, audit table) must NOT be
   created by the app at runtime. Provide an idempotent SQL script under
   scripts/sql/ for the DBA, and make the code fail with a clear message
   if the object is missing.
9. Don't change dropdowns, the resolver, sample-load, or reports.

Report ADDED/REMOVED (file:line), the on-screen flow, and behaviour for:
valid file, file with errors, duplicate ID "30"/"00030", Excel-numeric ID
cell, a 6-digit ID (accepted, stored as-is), an 11-digit ID (rejected),
missing header, extra columns, upload failure mid-way (old data intact),
empty file. Also the maintenance screen with a 6-digit ID (now allowed).
NULL cases: blank cells in each column. Build results. Do not commit.


Thanks Geoff. We'll pad shorter IDs to 5 digits and allow up to 10
digits, matching Ashok's column size, so future longer IDs won't break
the upload or the maintenance screen.
