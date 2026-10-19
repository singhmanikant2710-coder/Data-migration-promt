UAT #241 – CAS Linesheet PDF loses paragraph spacing in CAS Findings/Observations comments; Initial/Final Memo PDFs keep it.

Example: review 21795 (Integration Innovation Inc), findings UW-110, UW-114, CS-104. Initial Memo shows separate paragraphs; the CAS Linesheet (Review Summary, ReviewPDF.tsx) runs them together as one block.

PHASE 1 – Read-only discovery (file:line):
1. How do InitialMemoPDF.tsx / FinalMemoPDF.tsx render finding comments (HtmlRichText with availableWidth, from the #193/#194 work)?
2. How does ReviewPDF.tsx render the same comments in the findings table? Does it strip HTML to plain text, flatten <p>/<br>/<div>, or use HtmlRichText with different styles (e.g. no paragraph margin in table cells)?
3. List every other rich-text field in ReviewPDF.tsx (review form comment boxes) and whether each keeps paragraphs, lists, bold etc.
4. What does the stored HTML look like for these comments (Word paste with <p class=MsoNormal>, <o:p>, <br>, <div>)?

PHASE 2 – Smallest generic fix: the CAS Linesheet must render finding comments the same way as the memos (same shared renderer/rules), sized to its table column width. Generic for all reviews, not only 21795. No change to the memo PDFs or any other PDF unless Phase 1 shows a shared bug.

PHASE 3 – Implement only that.

PHASE 4 – Verify and report:
- Root cause (file:line).
- ADDED and REMOVED/CHANGED (file:line).
- Before/after for review 21795 in the CAS Linesheet.
- NULL/edge cases: comment NULL or empty; plain text with no HTML; single paragraph; <br>-only line breaks; Word-pasted HTML; very long comment that crosses a page break; tables/images in a comment (#193/#194 behaviour unchanged).
- Initial/Final Memo and other PDFs unchanged.
- tsc + frontend build, git status.
Do not commit or push.
