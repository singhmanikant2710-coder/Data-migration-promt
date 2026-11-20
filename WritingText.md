1: exclude rows where the CAS grade is NULL.
2: no highlighting.
3: same access as the other reports.
4: Sample_id and Review_id as numbers (consistent with #237/#256); Facility_number_calc, Account_number, Scorecard_id_bank, Customer_number stay text.
5: include rows where CAS is set and Bank is NULL: difference rule = (a.CAS_PD IS NOT NULL AND (a.Bank_PD IS NULL OR a.CAS_PD <> a.Bank_PD)) OR (a.CAS_LGD IS NOT NULL AND (a.Bank_LGD IS NULL OR a.CAS_LGD <> a.Bank_LGD)).
Proceed with Phase 3 and 4. Report ADDED/CHANGED (file:line), Dev row count vs a read-only SQL using the same rule, dotnet build + tests, npx tsc --noEmit, npm run build, git status (restore next-env.d.ts / tsconfig.tsbuildinfo). Do not commit or push.
