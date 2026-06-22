The single-review fetch runs multiple section queries 
(transactions, collateral, covenants, etc.), each with its own SQL. 
We keep hitting one wrong column at a time. I want to find ALL wrong 
columns at once.

Do NOT modify anything. Read all section-query methods in 
backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs 
that run during a single review fetch.

For EACH method, list:
- the method name
- the source table (e.g. dbo.[02_CORE_04_Accounts])
- every column referenced from that table (the a.[...] / alias.[...] names)

Output it as a plain list grouped by table, so I can cross-check 
every column name against the real database schema in one pass.
