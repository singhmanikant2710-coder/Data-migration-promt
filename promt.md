Geoff's side-question — 02_CORE_04_Accounts.Delinquent_status exports "1-30" as "30-Jan" (Excel misreading it as a date). Diagnose whether this is a DB storage issue or a CSV/Excel export formatting issue. READ-ONLY, no edits. One pass, answer, STOP.

1. What is the SQL data type of Delinquent_status in 02_CORE_04_Accounts? (Confirm via INFORMATION_SCHEMA.COLUMNS if you have DB access, or check any repository code that reads/writes this column for its declared SqlDbType.)
2. In the backend export query (ExportsController.cs, the Accounts export SELECT), is Delinquent_status selected as raw text, or is there any CAST/CONVERT applied to it?
3. Confirm: when the raw value read from the DB for this column is the string "1-30", does ToInvariantString (or EscapeCsv) do anything that could trigger Excel's date auto-detection — e.g. is the value written to the CSV WITHOUT quotes, making Excel free to reinterpret "1-30" as a date on open?
4. Is this a known CSV/Excel behavior (Excel auto-converts unquoted "N-NN" or "NN-Mon" patterns to dates on open, regardless of the source being plain text) that could be fixed by forcing the cell to be treated as text in the CSV (e.g. prefixing with a tab, wrapping in ="..."  formula-text trick, or quoting in a way Excel respects as text)?
5. Report: is Delinquent_status stored as NVARCHAR/VARCHAR in the DB (confirming this is Excel's export-side misinterpretation, not a data problem), or is there any evidence it's actually a DATE/DATETIME column?

Report the column's actual SQL type and whether this is a CSV-formatting fix or a DB data-quality issue. Do NOT propose a fix yet.
