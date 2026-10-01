Create test files for the Distribution Parties upload, so I can test it
manually in Dev. Test data only, no app code changes.

Put everything in test-data/dp-upload/ (do NOT commit; add the folder to
.git/info/exclude, not .gitignore). Create:

01-errors.csv: one row per failure type: blank email, invalid email,
   duplicate email (different case), blank name, blank Employee ID,
   non-numeric ID ("12A45"), 11-digit ID, duplicate ID "30" + "00030".
02-missing-header.csv: only "Name,Email" columns.
03-header-only.csv: headers, no data rows.
04-db-name-headers.csv: headers Recipient_email, Recipient_role,
   Recipient_name in a different order and mixed case, 3 valid rows.
05-extra-columns.csv: friendly headers + 2 extra columns (Title, Dept),
   3 valid rows.
06-valid-small.csv: 10 valid rows with a mix: 5-digit IDs with leading
   zeros ("08784"), a 3-digit ID ("30"), a 6-digit ID ("123456"), a name
   with a comma ("DOLCH, TERENCE J"), trailing spaces around values.
07-valid-small.xlsx: same 10 rows as 06, but the Employee ID column stored
   as Excel numbers with a "00000" number format, so it displays "08784"
   etc. One row: Employee ID as a plain number 30 (General format).
08-blank-rows.csv: 3 valid rows + 1 fully blank row (should be skipped)
   + 1 row with only Name filled (should be reported).

Use clearly fake data (e.g. test.user01@example-test.com,
"TESTUSER, ALPHA") so test rows are obvious and never match real people.

Also create:
- export-current-dp.sql: a SELECT that outputs the CURRENT Distribution
  Parties table in template format (Employee ID, Name, Email), so I can
  save it from SSMS as a realistic full-size file and add/remove a row for
  a real preview test.
- backup-restore-dp.sql: backup the live table into DP_backup_test before
  testing, and a restore section (in one transaction) to run afterwards.
- README.md: for each file, the exact expected on-screen result (error
  messages, which rows, preview counts), and a warning that 06/07 will
  REPLACE the whole Dev table with 10 rows, so run the backup first and
  restore afterwards.

Report the files created (paths + row counts) and confirm no app source
file was changed.
