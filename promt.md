READ-ONLY. Diagnostics only. Do not change anything.

Geoff reported the same "&amp; / &gt; / &lt;" jargon issue in the CAS 
Linesheet report (in addition to Initial/Final Memo). Plus a data error and 
a page-break issue. Investigate:

1. Locate the CAS Linesheet PDF component (likely CasLinesheetPDF.tsx or 
   similar in frontend/src/components/pdf/). Confirm the exact file path.

2. ENTITY ISSUE: Find where the CAS Linesheet renders long-text / narrative 
   fields that show "&amp;", "&gt;", "&lt;" literally (e.g. the "CRO concurs" 
   bullets, "Minimum DSC &gt;= 1.10x" covenant comments). Confirm whether it 
   routes this text through the SHARED HtmlRichText component (same as the 
   memos), OR through its own local parser/decoder. If it uses HtmlRichText, 
   the central decodeEntities fix will cover it too.

3. DATA ERROR — "Breaches Addressed" field: Locate where the CAS Linesheet 
   renders the "Breaches Addressed" field (in the Reporting/Monitoring 
   Covenants area). Geoff says it should display [Covenant_breaches_addressed] 
   from the CORE Reviews table. Show:
   - The current code/field that populates "Breaches Addressed" — what data 
     property is it bound to right now?
   - Whether the data model / repository already provides a 
     covenant_breaches_addressed / breachesAddressed field, and if so its 
     exact name.
   - Is the backend query for CAS Linesheet already selecting 
     Covenant_breaches_addressed from the Reviews table, or is it missing?

4. PAGE-BREAK ISSUE: Locate the "Unsatisfactory CRM Ratings" / "Risk Rating 
   Justification" section. Show whether it has a wrap={false} / break / 
   minPresenceAhead / "keep together" setting that forces the section to a 
   new page, pushing comments to the next page.

Do not edit anything. Findings only.
