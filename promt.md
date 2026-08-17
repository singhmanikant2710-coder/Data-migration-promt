READ-ONLY. Diagnostics only. Do not change anything.

Two report bugs: #181 (CRM Summary Table) and #182 (CRM Summary). Find their 
PDF component files first, then investigate all issues.

FIND THE FILES: Search for the two report components:
- CRM Summary TABLE report (#181) — likely CrmSummaryTablePDF.tsx or similar in 
  frontend/src/components/pdf/
- CRM Summary report (#182) — likely CrmSummaryPDF.tsx or similar
Show the file paths for both. (They may be separate files or share code.)

=== #181 — CRM Summary Table ===
1. HEADER: Show the current report header text and layout (title + run date). 
   Client wants: title "CRM Findings Summary Table" on the LEFT, run date on 
   the RIGHT, matching the PD Migration report's header layout. Show current 
   header structure. Also show the PD Migration report's header layout (as the 
   reference to match).
2. CRM COMPONENT HEADERS: Show the CRM Component section/table headers and 
   their current fontSize (client wants 11).
3. FOOTER: Show the current footer — its format and whether it includes 
   "Sample Name". Client wants footer: "[Report Name] - [Page # of ##]", with 
   the updated report name, and "Sample Name" removed. Show current footer code.
4. FILTER PAYLOAD: Show every place a "Current Filter Payload" section is 
   rendered. Client says there's a DUPLICATE on page 4 (remove it) plus the 
   final one on the last page (keep it, but rename heading to "APPLIED REPORT 
   FILTERS"). Show all filter-payload render locations so I can see the 
   duplicate vs the final one.

=== #182 — CRM Summary ===
1. HEADER FONT: Show the "CRM Summary" report header and its current fontSize 
   (client wants 14).
2. COLUMN HEADERS ALIGNMENT: Show the four column headers "Bank PD", "Bank LGD", 
   "CAS PD", "CAS LGD" (page 2 top) and their current textAlign (client wants 
   center).
3. SCORECARD ID WRAP: Show how the Scorecard ID is rendered (page 9) — its cell 
   width and wrap handling. It's not wrapping for long values (same issue we 
   fixed in memos with wrapAnywhere/wordBreak). Show the cell.
4. FILTER PAYLOAD HEADING: Show the "Current Filter Payload" heading (page 82) — 
   client wants it renamed to "Applied Report Filters" (text only, keep the 
   data).

Show all relevant code for both reports. Confirm whether #181 and #182 share 
any code (footer, filter payload, header components). Do not edit. Findings 
only.
