READ-ONLY. Diagnostics only. Do not change anything.

Geoff wants the CRM PD Grade Migration report footer to (a) remove the small 
FHN logo on the left, and (b) "mirror the CRM Summary Table footer using the 
current report name."

I need to see what the CRM Summary report's footer looks like so I can match 
it. Find and report (no edits):

1. Locate the CRM Summary report's PDF component (likely something like 
   CrmSummaryPDF.tsx or similar in frontend/src/components/pdf/). Show its 
   FOOTER code — the exact layout, what text/elements it renders (logo? 
   report name? page number? any brand text?), and the styles used.

2. Confirm whether the CRM Summary footer:
   - shows a logo or not
   - shows the report name (and how — hardcoded or dynamic)
   - shows "Page X of Y"
   - any other text (e.g. a brand/confidentiality line)

3. In CrmPdGradeMigrationPDF.tsx, re-confirm the CURRENT footer structure 
   (logo + "CAS RiskReview" + page number) and all the places it's repeated 
   (the diagnostics earlier noted ~5 footer blocks: first page, no-records 
   pages, detail pages, payload echo page). List each location.

4. Confirm how the current report name/title is available in 
   CrmPdGradeMigrationPDF.tsx (e.g. the metaTitle / data.meta.title / 
   "CRM PD Grade Migration" string) so I can use it in the footer.

Do not edit anything. Findings only.
