Bug in the Distribution Parties upload: row numbers are wrong in two ways.

1. Messages vs ROW column (01-errors.csv):
   - Row 5: "Duplicate email - already used on row 3" (should be row 4)
   - Row 11: "Duplicate Employee ID - the same ID is on row 9" (should be
     row 10)
   The ROW column adds +1 for the header, but the row referenced inside
   the message doesn't.
2. Skipped blank rows shift numbering (08-blank-rows.csv): line 5 is fully
   blank and is skipped client-side, so the name-only row on file line 6
   is reported as row 4.

Fix generically: every reported row number must be the ORIGINAL file line
number (header = line 1). Carry each row's original line number from the
parser to the backend (e.g. a LineNumber field on the upload row), and use
it for both the ROW column and any row referenced inside a message. No
+1/offset math anywhere else. The file-level error (e.g. "no data rows")
still displays as "-".

Report ADDED/REMOVED (file:line), and the corrected tables for 01-errors.csv
and 08-blank-rows.csv (every row number and message). Confirm that 02, 03,
04 and 05 behave as before. Build results. Do not commit.
