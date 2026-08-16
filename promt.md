READ-ONLY. Read once. Do not re-read.

File: InitialMemoPDF.tsx (confirm FinalMemoPDF.tsx matches)

Problem: the Account Info table header renders at the BOTTOM of a page with no 
rows beneath it (orphaned), then repeats on the next page with rows. This 
started after we made the header "fixed" and removed wrap={false} from the 
table container.

Show precisely:
1. The Account Information table structure NOW — the full skeleton:
   - The table container <View> (after we removed wrap={false})
   - The MAIN header row (does it have "fixed"? "wrap={false}"? styles.trHeader 
     with minPresenceAhead: 72?)
   - How data rows are rendered (the batch.map / rows loop)
   Show this skeleton exactly.

2. IMPORTANT: With "fixed", the header renders on EVERY page automatically. But 
   is there ALSO a non-fixed header being rendered once at the table start? 
   i.e. are there now effectively TWO header definitions (one fixed that 
   repeats, and the original inline one)? Show whether the header appears once 
   (fixed) or if there's a duplicate.

3. styles.trHeader — show it. Does it still have minPresenceAhead: 72? With a 
   fixed header, minPresenceAhead may not prevent the orphan.

4. CAS Linesheet (ReviewPDF.tsx) comparison — show its Account/Detail table 
   skeleton: how are rows grouped (chunked into separate <Page> components with 
   rowsPerPage?), and where is the fixed header placed relative to the rows? 
   The Linesheet does NOT orphan the header — show its exact structure so I can 
   match it.

Do NOT edit. Show the memo's current table skeleton (container + header + 
rows), whether there's a duplicate header, styles.trHeader, and the CAS 
Linesheet structure for comparison. Findings only.
