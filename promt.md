READ-ONLY. No edits. Read backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs and quote the CURRENT, ACTUAL RecomputeTtmCalculationsAsync method verbatim — the entire method from signature to closing brace, including the full SQL (required[] array, CTE, MERGE).

I need to see the REAL current state, because a diff was proposed that showed only 4 required columns and only 2 TTM columns in the CTE — but the actual method should have ~11 required columns and ~9 TTM component columns (curInterestExpenseTTM, curProfitBeforeTaxesTTM, curDepreciationTTM, curAmortizationTTM, curDistributionsTTM, curCPLTDTTM, curNetChargeOffTTM, curAveragePrincipalNRTTM). 

Quote the method EXACTLY as it currently exists. Do not summarize. Then STOP. No fix, no diff.
