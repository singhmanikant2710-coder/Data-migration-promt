Agreed, John. That flow is exactly where most of our testing and fixes have been focused. Summary of where it stands:

Add New Month (matches legacy)
- The new month is created with only the customer/fiscal fields, like legacy. Nothing is copied from the previous month: financial inputs start at 0, covenant actuals and custom fields are blank.
- Covenant definitions (name, order, threshold, format, reported) are seeded for the new month and placed in the correct covenant slots by order.
- Before the first Save, same-month values (EBITDA, Cash Avail for FC, FCC, Interest Coverage, GPM, TNW, D/TNW, Excess Availability) calculate live as values are entered; YTD/TTM fields stay blank until Save, as in legacy.

Monthly, YTD and TTM calculations on Save (verified against legacy)
- All 35 legacy calculated fields compared formula by formula with the Access expressions; mismatches fixed.
- YTD = running total of monthly values within the fiscal year; saving a month updates that month and the later months of the fiscal year (verified on Athens Paper by editing a month and checking every following month).
- TTM = calendar trailing 12 months, independent of fiscal year; saving a month recalculates that month and the following 11 months (e.g. Athens Paper 202510 PBT TTM corrected from 452 to 5,306, matching legacy).
- Fixed Charges TTM, FCC TTM, Interest Coverage TTM, A/R Turn Days, Inventory Turn, Net C/O TTM % and Reserve Coverage verified against legacy.
- Fiscal year/month assignment verified across different fiscal start months (January, May, June, October).

Also fixed along the way: industry templates for all 13 industries, covenant order/reported on the Customer page flowing to the Blackbook, custom fields displayed exactly as stored, values refreshing immediately after Save, and the PDF (Fiscal YTD vs Rolling 24 month ranges, formats).

Next: we're running the full sequence (Add New Month -> enter legacy values -> Save -> check monthly, YTD and TTM for that month and the following months) on one active customer per industry, and will send you the results with any differences marked as code or data.

One item still open on our side: changing a customer's fiscal start month does not yet recalculate existing months; until that is in, it's best not to change it during testing.

Happy to walk you or the business testers through it if that helps.
