Hi John, while comparing the new BCAT screens against legacy customer by customer, we found a few differences that come from the data in the SQL Server dev database rather than the application. Sharing them so they can be covered before the final data copy at cutover.

1. Blank values became 0
Where a numeric field is blank (NULL) in Access, the SQL Server copy holds 0. The screen then shows $0 where legacy shows blank, and 12-month averages count those zeros as real months.
Examples (202603):
- AMERICAN CREDIT ACCEPTANCE: Net C/O TTM $, Reserve Coverage, Cash Collections $, Net C/O $ - blank in Access, 0 in SQL.
- SHABANA MOTORS LLC 202601: Avg Principal N/R TTM - Access $44,413, SQL $4,412.50, because zero months are averaged.
Ask: when copying from Access, keep NULL as NULL (all numeric columns, all customers).

2. Custom field text changed
Custom fields are text and legacy displays them exactly as stored. In SQL the symbols were dropped and one value differs.
Example SHABANA MOTORS LLC 202601: Access "3.1%", "$2555", "3.67%", "2.79x" vs SQL "3.1", "2555", "3.67", "3.64".
Ask: copy custom field text exactly as stored in Access.

3. Customer data differs from legacy
MARINER FINANCE LLC 202603 in dev SQL does not match Access: Month PBT (SQL $9,757 vs Access $17,539), YTD PBT ($30,458 vs $35,584), Interest Coverage TTM (2.07 vs 2.73), the Net C/O basis (Gross N/R vs Principal N/R), and all covenants (SQL shows order 0 with no values; Access has orders 1-4 with values). It looks like an older or edited copy.
Ask: could this customer's data be refreshed from Access in dev, and confirm the cutover copy will come straight from Access?

Once the data is corrected, we will re-run the 12-month (TTM) recalculation on our side for the affected customers.

Happy to share the comparison queries if useful.
