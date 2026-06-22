The fix is confirmed. In dbo.[02_CORE_04_Accounts] the actual column 
is named 'Comp_call_description_system', but the query uses 
'Comp_call_description' (missing the '_system' suffix). This is the 
only cause of the 500 error.

Modify ONLY this one file:
backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs

In the GetTransactionsSectionAsync method, in the SQL SELECT list, 
change the single line:
    a.[Comp_call_description],
to:
    a.[Comp_call_description_system],

Rules:
- Change ONLY that one column reference in that one SQL string.
- Do NOT rename anything else, do NOT touch the C# property mapping 
  unless the reader maps strictly by column name and would break — 
  if so, STOP and tell me before changing it.
- Do NOT modify any other file under any circumstances.
- If you find the same wrong column name elsewhere in this method's 
  SQL, fix only within this method and tell me.

After the change, show me the diff.
