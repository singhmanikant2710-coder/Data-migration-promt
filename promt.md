Follow-up, same rules (build, tests, do not commit):

1. Slot override in TryMergeCovenantsIntoSeries: find the slot by
   matching tblMain.strCovenantName{N} to the covenant name first
   (trimmed, case-insensitive). Use intCovenantOrder only if no name
   matches. Reason: for ATHENS 202604/202605 in SQL Server, Min TNW is
   in slot 2 while slot 1 is "Other 1".
2. Bump payloadVersion "v20" -> "v21" (BlackbookSummaryService.cs:58).
3. SqlMainRepository 3178-3186 seeding probe: decide "anyActual" from
   dblCovenantActual{N} only, never from Formatted.
4. SqlCovenantRepository 420-437: when writing dblCovenantActual{N},
   also write dblCovenantActual{N}Formatted in the same statement
   (legacy: "$" -> "$" + #,##0, otherwise FormatNumber(x,2) + format),
   and NULL both when the actual is NULL.


   SELECT m.strMonthKey
FROM tblMain m
WHERE m.strCustomerName LIKE 'ATHENS PAPER%'
  AND m.strMonthKey BETWEEN '202501' AND '202612'
  AND NOT EXISTS (SELECT 1 FROM tblMainCovenants c
                  WHERE c.strCustomerName = m.strCustomerName
                    AND c.strMonthKey = m.strMonthKey)
ORDER BY m.strMonthKey;
