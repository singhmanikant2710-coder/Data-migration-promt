READ-ONLY. Diagnostics only. Do not change anything.

In frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx, after removing 
the <FiltersEcho> (Selection Summary) section, there is now a large empty 
gap on page 1: the "PD GRADE MIGRATION BY SCORECARD COUNT" heading appears, 
then most of the page is blank, and the actual matrix table is pushed to 
the next page.

Show me:
1. The JSX from the DistCharts / "PD DISTRIBUTION" charts block down to and 
   including the "PD GRADE MIGRATION BY SCORECARD COUNT" section and the 
   matrix table that follows it.
2. Any of these that could create the gap between the heading and its table:
   - a <View break /> or `break` prop
   - a wrapping <Page> boundary (is the heading on one Page and the table 
     on a different Page component?)
   - a fixed height, minHeight, marginTop, or paddingTop on the section 
     wrapper or the heading
   - a leftover empty <View> where FiltersEcho used to be, still holding 
     its own height/margin
3. Confirm whether the "PD GRADE MIGRATION BY SCORECARD COUNT" heading and 
   its data table are inside the SAME <Page> or split across two <Page> 
   components (which would force the table to the next page and leave the 
   heading stranded with whitespace).

Do not edit anything. Findings only.
