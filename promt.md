Context: CASRR monthly Data Mart upload (/admin/monthly-upload →
frontend/src/app/api/monthly-upload/{parse,save}) loads a CSV into
[01_DATA_01_Data Mart Trial]. CompCallCode is a text field, but values like
"01E0", "01E1", "01E2" are stored as "010" (parsed as scientific notation).
This column maps to Comp_call_code_system / Comp_call_code_CAS in
[02_CORE_04_Accounts] at sample load. A similar bug was fixed earlier for
DelinquentID ("1-30").

1. Investigate: find exactly where the conversion happens (CSV parser dynamic
   typing, Number()/parseFloat, the save mapping, or the SQL insert) and how
   the earlier DelinquentID fix was done.

2. Fix generically: every column that is nvarchar/text in
   [01_DATA_01_Data Mart Trial] must be kept as the raw string exactly as in
   the file — leading zeros, E-notation-looking values ("01E0", "1E5") and
   dash ranges ("1-30"). Don't patch CompCallCode alone; let the table schema
   drive text vs numeric where possible, not a hand-maintained list.
   Genuinely numeric target columns (float/decimal: amounts, rates, PD/LGD,
   delinquency counts) must parse exactly as before.

3. Check whether the upload accepts .xlsx. If yes, apply the same rule for
   xlsx (text columns read as text). If not, report what it would take;
   don't add it in this change.

4. Verify WITHOUT the upload screen (I don't have access to it) and WITHOUT
   writing to any database: create a temporary test CSV with columns
   CompCallCode, DelinquentID and one numeric amount column, rows:
   "01E0", "01E1", "01E2", "0012", "1E5", empty, plus DelinquentID "1-30"
   and amount "1234567.89". Run it through the SAME parse function and save
   mapping (up to building the insert values) that the real upload uses.
   Print each cell's value + JS type, before vs after the fix. Delete the
   temp files afterwards.

Report:
- Root cause (file:line)
- ADDED / REMOVED lines (file:line)
- Before/after table from step 4 for every test value, including the empty
  cell (must become NULL/empty exactly as today, never "0")
- Which columns are now treated as text vs numeric
- xlsx finding
- Build result
Do not commit.
