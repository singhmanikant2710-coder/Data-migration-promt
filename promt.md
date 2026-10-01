Hi Geoff,

They can be different. Simple headers are best: "Employee ID", "Name",
"Email". We'll map them to the table columns internally. Column order won't
matter and headers will be case-insensitive. We'll also provide a template
download on the upload page.

Thanks,
Manikant

Context: CASRR. Build the Distribution Parties upload, per the client:
- Admin uploads the full bank employee file (Name, Email, Employee ID,
  ~15k rows), CSV or XLSX, monthly/quarterly.
- Each upload FULLY REPLACES dbo.[03_LIBRARY_10_Distribution Parties]
  (departed associates drop off). Maintenance-screen edits are a temporary
  bridge and are replaced by the next upload.
- Column headers: [paste from Geoff].
Use your earlier investigation as the design base.

Requirements:
1. New admin page (same admin gate + [Authorize(Policy="RequireAdmin")])
   with a downloadable template, parse → validate → preview → confirm.
2. Read Employee ID as TEXT in both CSV and XLSX (same lesson as
   CompCallCode). For XLSX cells that Excel already stored as numbers,
   pad numeric values to 5 digits; reject anything non-numeric or
   longer than 5 digits.
3. Whole-file validation before any write, all errors listed per row:
   blank/invalid email, duplicate email (case-insensitive), blank name,
   blank/non-numeric/>5-digit ID, duplicate ID compared numerically
   ("30" = "00030"). Any error → nothing written.
4. Preview diff vs current table (key = email): added / removed / changed
   (ID compared numerically) / unchanged count. Show the removed list.
5. Atomic replace in ONE backend call and ONE transaction; on any failure
   the old table stays intact. Prefer staging table + swap, so readers
   (NOLOCK) never see an empty table. Pad IDs to 5 digits in the loader
   AND use SqlBulkCopyOptions.FireTriggers.
6. Audit row: who, when, file name, counts added/removed/changed.
7. Don't change the maintenance screen, dropdowns, resolver, sample-load,
   or reports.

Report ADDED/REMOVED (file:line), on-screen flow, behaviour for: valid
file, file with errors, duplicate ID "30"/"00030", Excel-numeric ID cell,
upload failure mid-way (old data intact), empty file. NULL cases: blank
cells in each column. Build results. Do not commit.


Context: CASRR. Client wants XLSX support (in addition to CSV) for:
1. Data Mart Trial monthly upload: the API already accepts .xlsx, but the
   browser input (admin/monthly-upload page.tsx:511) only accepts .csv.
   Enable .xlsx end to end.
2. Sample file upload (currently CSV only): add .xlsx with identical
   parsing and validation.
3. CompCallCode: the Data Mart fix keeps it as text on upload. Verify it
   stays text through sample load into [02_CORE_04_Accounts]
   (Comp_call_code_system / Comp_call_code_CAS): column types, the INSERT
   expression, and any conversion. Fix only if it's coerced.

Rules (generic):
- Text columns must keep the raw value (leading zeros, "01E0", "1-30").
- If Excel already stored a text-column cell as a number (e.g. 01E0 saved
  as 1), reject the file with a clear "format the column as Text" message
  for that column + row, same pattern as the DelinquentID guard.
- Numeric columns parse exactly as today. CSV behaviour must be unchanged.

Report ADDED/REMOVED (file:line), behaviour for CSV and XLSX with:
"01E0", "0012", "1-30", empty cell, a numeric amount, and an Excel-numeric
cell in a text column. Confirm the Accounts columns' types and the end-to-
end CompCallCode value. Build results. Do not commit.
