READ-ONLY. Read ONE block only from backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs: the perCashCollections computation block. Read ONCE, quote, stop. Do NOT read other files. Do NOT loop.

Quote the CURRENT perCashCollections block exactly as it is in the file right now — the full if(Num("perCashCollections")...) block including the denCash CASE and the sets.Add. I need to see:
1) Does it use curPrincipalNRPriorMonth or curPrincipalNR in the denominator right now?
2) What is the exact LIKE pattern — 'PRINCIPAL' or 'PRINCIPAL%'?

Context: DB now has perCashCollections=5 for this row (500000/100000=5), which means the denominator being used is curPrincipalNR (current=100000), NOT curPrincipalNRPriorMonth (prior=1000000). So either the prior-month change didn't apply, or the LIKE branch fell through to a current-month path.

Output only: the current perCashCollections block, verbatim. Then STOP.
