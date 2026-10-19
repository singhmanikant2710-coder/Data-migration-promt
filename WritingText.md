UAT #241 – regression: CAS Linesheet for review 21795 now shows the CRM Findings header but NO rows (UW-110, UW-114, CS-104 all missing). The PDF dropped from 14 pages to 4. Initial Memo for 21795 still shows them.

Also review 21592: LA-101 comment in the Linesheet breaks lines at odd places ("...Term Loan was not closed" / "as approved and..."), while the Initial Memo wraps the same text normally.

PHASE 1 – Read-only (file:line):
1. Why are the rows dropped? Confirm whether a findings row with wrap={false} taller than the available page height is omitted by react-pdf, and whether one oversized row also removes the rows around it. Measure/estimate the row heights for 21795 in the 42% column.
2. Do the Initial/Final Memo findings tables have the same latent risk with a long enough comment (they use wrap={false} too)?
3. 21592 LA-101: what is the stored comment (plain text with \n / \r\n, or HTML)? Why does the Linesheet show hard line breaks that the memo does not – are both PDFs really using the same path for plain text?

PHASE 2/3 – Smallest generic fix in ReviewPDF.tsx:
- A findings row must NEVER disappear. Rows that fit on a page stay unbroken as today; a row taller than the page must be allowed to break across pages (keep the table header repeated if the table already does that, and avoid an orphaned 1-2 line fragment, e.g. minPresenceAhead).
- Plain-text comments must render exactly as in the Initial Memo (same line-break handling).
- If Phase 1 shows the memos have the same latent risk, report it with file:line but do NOT change the memos in this ticket.

PHASE 4 – Report: root cause for both issues, ADDED and REMOVED/CHANGED (file:line), before/after for 21795 and 21592, edge cases (short comments; one huge comment; several long comments; comment NULL; table starting near the page bottom), confirm no other section of the Linesheet changed, tsc + build, git status.
Do not commit or push.
