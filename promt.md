READ-ONLY. Read once. Do not re-read.

File: frontend/src/components/pdf/ReviewPDF.tsx (CAS Linesheet)

Issue: the fixed table header (Account Information / Scorecard tables) renders 
at the BOTTOM of a page with no data rows beneath it (orphaned), then repeats 
on the next page. This is the "fixed" prop rendering the header on every page 
even when the table content has ended or hasn't started on that page.

Show precisely for the Account Information table (and Scorecard Assessment 
table if same pattern):
1. The exact table structure: the <View style={styles.table}> container, the 
   <View ... fixed> header row, and the rows.map. Show if the header's "fixed" 
   is scoped to the table View or the whole Page.
2. CRITICAL: In react-pdf, "fixed" on a View inside a table renders it on 
   EVERY page of the DOCUMENT/Page, not just pages the table spans. Is the 
   header's fixed causing it to render on pages AFTER the table has ended (or 
   before it starts)? Show what comes AFTER the Account Info table (next 
   sections) — does the fixed header bleed onto those pages, OR is the orphan 
   specifically at the table's own page-boundary?
3. Show whether the table is the last content or has sections after it. Show 
   the surrounding section order.
4. Is there any minPresenceAhead or wrap handling on the header or rows that 
   could keep the header with at least one row?

I need to understand if the orphan is: (a) fixed header bleeding onto 
non-table pages, or (b) header rendering at a page-bottom right at the table's 
break point. Show the structure and surrounding context. Findings only. Do NOT 
edit.
