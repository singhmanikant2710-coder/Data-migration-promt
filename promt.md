BUG: Add New Month seeds tblMainCovenants with strCovenantReported =
NULL (ATHENS 202604: Other 1 (%), Min TNW, Min Net Income all NULL).
Legacy 202604 has Monthly / Quarterly / Quarterly — legacy seeds covenant
definitions for a new month from the tblCustomer template
(strCovenant{X}Name/Reported/Order/Threshold/Description/Format,
Form_frm001TruckingMain.bas:352-433).
Attached: 202603 tblMainCovenants Reported values and tblCustomer
template Reported values for ATHENS.

1. READ-ONLY: quote SeedFromLatestAsync's clone INSERT — is Reported
   copied, and from where? Why is it NULL for 202604?
2. FIX (smallest): on a new month, strCovenantReported comes from the
   tblCustomer template for the covenant with the same name (legacy
   source). If no template match, fall back to the latest month's value.
   Do not change how name/order/threshold/format/actual are seeded.
RULES: new-month seeding only; existing months unchanged.
GOLDEN: ATHENS new 202604 -> Other 1 (%) Monthly, Min TNW Quarterly, Min
Net Income Quarterly; covenant actuals still NULL; ATHENS 202603
unchanged; one other industry add-month has Reported filled.
Build, tests, do not commit.
REPORT: root cause (file:line), ADDED/REMOVED, BEHAVIOUR CHANGE incl.
NULL, NOT TOUCHED.

