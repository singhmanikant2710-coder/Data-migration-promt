Hi Geoff,
Thanks for the confirmations earlier — I've rebuilt the Non-Compliant Covenants report to match. A few things to flag and one quick question before I finalize:
1. Excel export — do you need it too?
Your ticket was about the report (PDF), which I've rebuilt. There's also an "Export to Excel" option on the Reports screen. Right now, exporting Non-Compliant Covenants to Excel produces the wrong workbook (it falls back to the Covenants Summary export — a pre-existing issue, not something my changes caused).
Do you need the Excel export for Non-Compliant Covenants as well? If so, should it contain the same data as the new PDF (totals + monitoring/performance breakdown + details), or just the detail rows? If Excel isn't needed for this report, I'll leave it as-is.
2. Confirming the COUNT column meaning
In the "Monitoring Covenant Violation Totals" and "Performance Covenant Violation Totals" tables, the COUNT column represents distinct borrowers, not the number of covenant rows. So if one borrower has two covenant types, they're counted once per type in their respective rows, but the "Totals (N borrowers)" line counts them as one borrower. This matches your prototype (e.g. Budget $9.7M + AP Aging $23.4M = $33.1M total across 2 borrowers). Just confirming this is the intended behavior.
3. Statuses included
Per your note, the report now includes 'Not Compliant' and 'Past Due' in both the summary totals and the details table. I did not include 'Waived' — please confirm that's correct (you mentioned Not Compliant and Past Due, but not Waived).
Everything else is done — the routing bug is fixed, the header/footer follow the house standard (report name + page number, no logo), the 8 detail columns are in the order you specified, THRESHOLD/RESULT are blank for Monitoring covenants, and exposure is based on Commitment. I'll do a final test against a real sample once you confirm the above.
Thanks!
