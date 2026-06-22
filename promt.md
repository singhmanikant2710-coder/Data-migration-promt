Final column fix. In dbo.[02_CORE_02_Reviews] the actual column is 
named 'Covenant_validation_accuracy', but the code uses the wrong 
name 'Covenant_calculation_accuracy'. This is the last invalid column.

First, search the ENTIRE backend Infrastructure layer for the exact 
string "Covenant_calculation_accuracy" to find every occurrence.

Then, in each place it appears inside a SQL query against 
02_CORE_02_Reviews, change:
    Covenant_calculation_accuracy
to:
    Covenant_validation_accuracy
(keep any AS alias exactly the same — only change the source column name)

Rules:
- Modify ONLY the column name in the SQL string(s). 
- Do NOT change any C# property names, aliases, or mapping.
- Do NOT modify any file outside backend/src/Casrr.Infrastructure/.
- If the reader maps this column by name (not ordinal) and renaming 
  the source would break it, add "AS Covenant_calculation_accuracy" 
  to preserve the output name, and tell me you did so.
- Show me the diff after.
