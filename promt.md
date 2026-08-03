READ-ONLY. Diagnostics only. Do not change anything.

I want to refactor the CRO footer to match PD Grade's WORKING structure so 
the `render` prop (page numbers) fires. The render prop only works when 
<View fixed> is a DIRECT child of <Page>. I need to see CRO's exact page 
structure to find why render fails.

Show me VERBATIM:

1. In CrmPdGradeMigrationPDF.tsx (WORKING): the full return() of the page 
   component (CrmPdGradeMigrationPage) — from the opening <Page ...> to 
   </Page>, showing the DIRECT children of <Page> and exactly where the 
   footer <View style={styles.footer} fixed> sits. I need to see the nesting 
   depth: is the footer a direct child of <Page>, or nested inside another 
   <View>?

2. In CroProductionSummaryPDF.tsx (NOT working): the full return() of 
   CroProductionSummaryPage — from opening <Page ...> to </Page>. Show ALL 
   the direct children of <Page> and exactly where the footer sits. 
   SPECIFICALLY: is the content wrapped in an outer <View> (e.g. a scroll/
   wrapper container) that ALSO contains the footer? Is the footer nested 
   inside that wrapper <View> rather than being a direct child of <Page>?

3. Compare the nesting: 
   - PD Grade: <Page> → [content views] + <View footer fixed> (footer direct 
     child of Page?)
   - CRO: <Page> → <View wrapper> → [content] + <View footer fixed>? (footer 
     nested inside a wrapper, NOT direct child of Page?)

4. Also show the second <Page> in CRO's default export (the payload page) — 
   same nesting question for its footer.

5. Confirm: does CroProductionSummaryPage wrap its entire body in a single 
   outer <View> (with something like flex/height styles) that the footer is 
   trapped inside? That would break the fixed/render page context.

Report the EXACT nesting difference between the working PD Grade footer 
placement and the broken CRO footer placement. Findings only.
