READ-ONLY. Diagnostics only. Do not change anything.

CRITICAL COMPARISON: The CRM PD Grade Migration report footer 
(CrmPdGradeMigrationPDF.tsx) WORKS — it shows "CRM PD Grade Migration • Page X of Y". 
The CRO Review Production footer (CroProductionSummaryPDF.tsx) does NOT render 
its text, even though we applied a similar fix. Both are PDF reports from the 
same Reports dropdown. There must be a structural difference. Find it.

Compare these EXACT things between CrmPdGradeMigrationPDF.tsx (WORKING) and 
CroProductionSummaryPDF.tsx (NOT working):

1. The WORKING footer block in CrmPdGradeMigrationPDF.tsx — show it verbatim. 
   (Note: earlier this footer was changed to a centered "${title} • Page X of Y" 
   and it renders correctly.) Show its exact structure: is there an inner View? 
   Is `render` a prop? What is the parent chain?

2. How is the footer's `title` / report-name variable obtained in the WORKING 
   CrmPdGradeMigrationPDF footer vs the CRO footer? Show the exact variable 
   and where it comes from in EACH. (e.g. is it a prop, a const, a literal?)

3. The <Page> and <Document> structure around the footer in BOTH files:
   - In CrmPdGradeMigrationPDF: how is the page component defined, and does 
     the footer sit inside <Page>...</Page> directly?
   - In CroProductionSummaryPDF: same — show the <Page> wrapper and where the 
     footer sits relative to it.
   - KEY: Is the footer inside the SAME <Page> element that has the content, 
     or is it structured differently (e.g. CRO uses a separate component 
     CroProductionSummaryPage that is placed inside a <Page> at the call site, 
     vs PD Grade defining <Page> inside its page component)?

4. Does the WORKING PD Grade footer <Text> use `render`, or does it use 
   static children with the
