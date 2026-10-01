Haan, “you’re very senior and respectful to me” thoda unnatural lag raha hai. Better and more professional:

> No issue, Geoff. Please don’t say sorry. I have a lot of respect for you, and I genuinely enjoy working with you because of your support, guidance, and the way you manage the team. That’s why I’m giving 100% effort to make sure everything is completed as promised.

I’ll try to complete the regression tomorrow and push the changes. If I’m unable to complete it tomorrow, I’ll definitely push the changes to QA on Monday before you log in.
Decision: RECOVER, with targeted guards. Build part 2 and the guard together.

1. Generic rule for text columns in XLSX (Data Mart + Sample files): use
   the cell's formatted display text (ExcelJS cell.text) when the raw value
   is numeric, via ONE shared helper used by both parsers. Large integers
   must never come out in scientific notation (e.g. 4013430000000 stays
   "4013430000000", not "4.01343E+12").
2. Targeted reject-with-hint only for closed-domain/code columns where a
   numeric cell means data was lost:
   - DelinquentID: keep the existing guard unchanged.
   - CompCallCode: if an XLSX cell is numeric (not text), reject the file
     with "CompCallCode must be formatted as Text in Excel (row N)".
   Keep these columns in one small list next to the helper.
3. Sample file xlsx (part 2): refactor onUploadFile so the existing
   splitCsvLine validation loop is shared unchanged between CSV and XLSX.
   customer_number goes through the recover helper.
4. CSV behaviour must be byte-identical to today on both uploads. Numeric
   columns parse exactly as today.

Report ADDED/REMOVED (file:line), and a CSV vs XLSX behaviour table for:
"01E0" as text, CompCallCode as an Excel number, "0012" (text and as a
"00000"-formatted number), "1-30", empty cell, numeric amount, a 13-digit
account number, and customer_number in a sample file. Empty cell must
stay NULL, never "0". Build results. Do not commit.
