READ-ONLY. Diagnostics only. Do not change anything.

Context: The "PD/LGD Grade Migration" (aka "CRM PD Grade Migration") PDF 
report needs several fixes. I need to locate the exact code for each before 
making changes. This is generated when the user picks that report on the 
Reporting screen and clicks "Generate PDF Report".

Find and report (no edits):

1. PAGE SIZE / ORIENTATION: Where is the PDF page size and orientation set 
   for this report? Report should be 11 x 8.5 inches Landscape. Show the 
   current page/size/orientation config and the file.

2. REPORT HEADER: Locate the header that currently renders 
   "<sample> - <date> - Examination - ... - <run timestamp>". Show the code 
   that builds this header string. I need to change it to show ONLY the 
   run date in MM/DD/YYYY format (no time, no sample name, no sample 
   details).

3. HEADER FONT COLOR: Show the CSS/style controlling the header text color 
   (it needs to be White).

4. SELECTION SUMMARY SECTION: Locate the "SELECTION SUMMARY" block 
   (Sample, Segment, Unit, Market, RM, PM, Start Date From/To, Finalized 
   From/To, Approved Only, Exclude Unchanged). This whole section must be 
   removed for this report. Confirm whether this section is specific to the 
   PD/LGD Grade Migration report or shared across multiple reports.

5. PD DISTRIBUTION CHART DATA: Locate the query/logic that populates 
   "PD DISTRIBUTION BY SCORECARD COUNT". It currently counts scorecards. 
   Geoff wants it to count Number of Accounts instead. Show the current 
   query/aggregation and confirm what table/column distinguishes "scorecard 
   count" vs "account count" (likely dbo.[02_CORE_04_Accounts]).

For each of the 5, report: the exact file path, the method/template/config, 
and whether the code is specific to this report or shared with other 
reports. Do NOT edit anything. Findings only.
