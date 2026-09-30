Hi Jay, thanks for checking. Agreed, Keystone's fiscal start month (October, 9/30 year end) is correct in both systems. The difference is the fiscal year number stored on each month, and it is the same in legacy Access and in the new system's data.

For an October start, the fiscal year is named for the year it ends, so October 2025 to September 2026 is FY2026. That is how every other October-start customer is stored (e.g. ATHENS PAPER: 202104 = FY2021, 202110 = FY2022).

Keystone is stored one year higher on every month, in legacy and in the new system:
- 202104 = FY2022 (standard would be FY2021)
- 202110 = FY2023 (standard FY2022)
- 202510 to 202605 = FY2027 (standard FY2026)

So the FY2027 months did transfer. The new system places them under FY2026, which is why FY2027 looks empty there. This is one of the four customers John's team planned to correct in the data before cutover. Could you confirm that FY2026 is the correct label for October 2025 to September 2026 for Keystone?


Hi John,

Point 5: you're right. Of the 44 customers with no industry, only TEMPO GLOBAL RESOURCES FKA HUNTER DOUGLAS has Black Book records (18 months). That one needs an industry set; the rest can be ignored.

Point 3, with detail: the dev SQL copy is missing monthly records (tblMain) that exist in Access. The covenant rows for those months did load, which is how we spotted it.
- ALAN WIRE COMPANY: Access has every month 202505 to 202603; dev SQL is missing 202508 to 202512.
- ADIR INTERNATIONAL LLC: Access has 202401 to 202404; dev SQL has none of them.
- WORLD ACCEPTANCE (202508), ALLSTATES WORLDCARGO (202609) and MDR CONSTRUCTION (202601) show the same pattern and are likely the same issue.
- BANKERS HEALTHCARE GROUP (covenant rows for future months 202607 to 202701) and THUNDER CARRIER SERVICES (one covenant row with a blank month) look like leftover rows.

Could the DBA check why these monthly rows did not load, and compare the tblMain row count per customer between Access and SQL so we know nothing else is missing before cutover? The query that lists these cases:
SELECT DISTINCT c.strCustomerName, c.strMonthKey
FROM tblMainCovenants c
WHERE NOT EXISTS (SELECT 1 FROM tblMain m
  WHERE m.strCustomerName = c.strCustomerName AND m.strMonthKey = c.strMonthKey)
ORDER BY c.strCustomerName, c.strMonthKey;
