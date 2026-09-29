Hi John, one more data item, and this one looks more serious than the rest.

ALAN WIRE COMPANY has monthly data in Access for every month from 202505 to 202603 (FY2026). In the dev SQL copy, tblMain is missing 202508 through 202512 (fiscal months 4 to 8). The covenant rows for those months did come across into tblMainCovenants, so it looks like the monthly rows were dropped during the load rather than never existing.

The same pattern (covenant rows present, monthly row missing) also shows up for:
- ADIR INTERNATIONAL LLC: 202401 to 202404
- WORLD ACCEPTANCE CORPORATION: 202508
- ALLSTATES WORLDCARGO INC: 202609
- MDR CONSTRUCTION INC: 202601
- BANKERS HEALTHCARE GROUP LLC: 202607 to 202701 (future months, may be leftover rows)
- THUNDER CARRIER SERVICES LLC: one row with a blank month key

Could the DBA check whether these tblMain rows exist in Access and, if so, why they did not load? It would also be worth confirming the full row count per customer between Access and SQL, so we know no other customer is missing months before cutover.

This query lists every case in SQL:
SELECT DISTINCT c.strCustomerName, c.strMonthKey
FROM tblMainCovenants c
WHERE NOT EXISTS (SELECT 1 FROM tblMain m
  WHERE m.strCustomerName = c.strCustomerName AND m.strMonthKey = c.strMonthKey)
ORDER BY c.strCustomerName, c.strMonthKey;

Thanks.
