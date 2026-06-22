Good. Now do a thorough audit before I rebuild. Search the ENTIRE 
backend Infrastructure layer for SQL column references that are 
missing the '_system' suffix but where the real DB column ends in 
'_system'.

Specifically, search all .cs files in 
backend/src/Casrr.Infrastructure/ for these exact wrong column 
names (without _system) used inside SQL strings on the 
dbo.[02_CORE_04_Accounts] table:
- Comp_call_description  (should be Comp_call_description_system)
- Collateral_description (should be Collateral_description_system)

Also list ALL columns selected from dbo.[02_CORE_04_Accounts] 
(alias a) across every method in the Infrastructure layer, so I 
can cross-check them against the real table schema.

Do NOT modify anything beyond the two confirmed fixes. Just report 
the full list of a.[...] columns used against 02_CORE_04_Accounts.
