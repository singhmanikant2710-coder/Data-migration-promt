Bug in the Distribution Parties upload error table: row numbers inside the
messages don't match the ROW column. Uploading 01-errors.csv shows:
- Row 5: "Duplicate email - already used on row 3" (should be row 4)
- Row 11: "Duplicate Employee ID - the same ID is on row 9" (should be
  row 10)
The ROW column is the file line number (page adds +1 for the header), but
the row referenced inside the message is the 0-header data-row number.

Fix generically: use ONE numbering everywhere, the file line number
(header = line 1, first data row = line 2). Do the offset in one place
(the backend, so messages and the row field agree), and remove the +1 in
the page so it isn't applied twice. The file-level error (row 0, e.g.
"no data rows") must still display as "-".

Report ADDED/REMOVED (file:line), the corrected table for 01-errors.csv
(every row number and message), and confirm the empty-file and
missing-header cases are unchanged. Build results. Do not commit.
