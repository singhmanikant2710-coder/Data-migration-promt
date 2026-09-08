The latest screenshot shows that the Policy Exception Details and Exception Description data are now correct.

However, there is still an aggregation issue in the POLICY EXCEPTION TOTALS table.

Current result:
- E08 - Speculative Land Loan → COUNT = 1
- E17 - Real Estate - Minimum Equity → COUNT = 1
- Totals → COUNT = 1 ❌

The total COUNT should be 2 because:
1 + 1 = 2.

Please compare this with the attached prototype screenshot and inspect the backend aggregation logic that builds the Policy Exception Totals table.

Important:
- The two exception rows belong to the same Review ID (21748), so do not blindly change unique-borrower logic.
- The COUNT column in the totals table should be the sum of the exception counts by exception description/type.
- Therefore, for this data, the totals-table COUNT must be 2.
- If the report separately displays a "Total Borrowers" value, that should continue to follow the prototype's unique-borrower definition. Do not mix unique borrower count with total exception count.

Please identify exactly why the totals row is currently returning COUNT = 1 instead of 2, fix only the aggregation logic required for this issue, and ensure the exposure total remains correct.

Expected result for the attached screenshot:

EXCEPTION DESCRIPTION                         COUNT
E08 - Speculative Land Loan                     1
E17 - Real Estate - Minimum Equity              1
---------------------------------------------------
Totals                                           2

Please do not change the already-correct Exception Description or Policy Exception Details rendering.
