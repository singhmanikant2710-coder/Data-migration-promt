READ-ONLY. Blackbook PDF (report page) shows YTD Sales and YTD PBT = 0
for ATHENS PAPER 202504-202509 only. tblMain has correct non-zero
curRevenueOrSalesYTD / curProfitBeforeTaxesYTD for those months, and
the edit-page UI shows them correctly. Other months in the PDF are fine.
Note 202504-202509 are fiscal months 7-12 of FY2025 (Oct start).

Trace for the PDF: which endpoint(s) feed those rows, how the YTD
Sales / YTD PBT cells are resolved (aliases, fiscal-year grouping,
recompute from monthly, series merge between years), and quote the
file:line that turns them into 0. Also say whether any change made in
this session caused it. Report only.
