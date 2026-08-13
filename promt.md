READ-ONLY. Read once. Do not re-read.

File: frontend/src/components/pdf/ReviewPDF.tsx (CAS Linesheet)

The Transaction Information section moves to the top of the next page, leaving 
empty space on the previous page (Geoff's "EMPTY SPACE" comment). Diagnostics 
showed the section itself has no wrap={false}/break/minPresenceAhead. Confirm 
the cause:

1. Show the section IMMEDIATELY BEFORE Transaction Information in the render 
   order — does it have break, wrap={false}, or minPresenceAhead that could 
   push Transaction Info down? Show that preceding section.

2. Show styles.section and styles.sectionTitle — do they have any 
   minPresenceAhead, break, or wrap that keeps the title+content together 
   (which would push the whole section to the next page if the title can't fit 
   with some content)?

3. The HtmlRichText inside Transaction Info — is the whole section wrapped in 
   anything that prevents splitting? Could the section's content be large 
   enough that React-PDF keeps title+content together and moves both down?

4. Is there a <View break> anywhere near/before Transaction Information (the 
   diagnostics mentioned "Unsatisfactory CRM Ratings" has a break) — could a 
   forced break earlier be causing the empty space?

Do NOT edit. Show the preceding section, styles.section/sectionTitle, and any 
break/wrap near Transaction Info, so I can tell if this is a fixable forced 
rule or natural pagination. Findings only.
