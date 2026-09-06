Bug 202 fix — Option A, minimal & safe. Make all three screens (Review Queue, Review History, Review Progress) open the CAS Linesheet PDF with the SAME icon and the SAME tooltip "Open CAS Linesheet PDF". Only touch what's actually inconsistent. Show all diffs before applying.

Review Queue and Review History ALREADY open the PDF correctly with the shared file-text SVG — do NOT change their PDF-open behavior, modal, data, or download logic. Only update their tooltip strings.

Changes:

1. Review Queue (app/review-queue/page.tsx, ~lines 441-442): change title and aria-label from "Open Review Summary PDF" → "Open CAS Linesheet PDF". Nothing else.

2. Review History (app/review-history/page.tsx, ~lines 150-151): same — title/aria-label → "Open CAS Linesheet PDF". Nothing else.

3. Review Progress (app/review-status/page.tsx) — this is the real change. Currently its DocIcon (~lines 21-28) is a <Link> that navigates to Review Info and does NOT open a PDF. Change it to open the CAS Linesheet PDF exactly like Review Queue does:
   a. Replace the DocIcon glyph with the SAME 5-sub-path 16x16 file-text inline SVG used by Review Queue (~lines 444-450) / Review History (~lines 153-159), so all three icons match.
   b. Add the PDF-open behavior by mirroring Review Queue's exact pattern: import ReviewPDFModal, add the pdfRow state, turn the icon into a <button> (size xs, ghost) with onClick that sets pdfRow for that row, and render <ReviewPDFModal> the same way Queue does (~lines 931-936), passing the same borrower/review data shape Queue passes. Do NOT invent a new data flow — copy Queue's wiring.
   c. Tooltip: title/aria-label = "Open CAS Linesheet PDF".
   d. IMPORTANT: the row's OTHER links that legitimately navigate to Review Info (the Review Id link ~line 621 and borrower name link ~line 642, tooltip "Open Review Info") must STAY as navigation — do NOT change those. Only the document/PDF icon changes.

Do NOT refactor Queue/History to a shared component. Keep changes minimal. Do NOT change ReviewPDFModal, download filenames, or backend.
List every file + line changed. Commit: "Fix Bug 202: all three review screens open CAS Linesheet PDF with consistent icon and 'Open CAS Linesheet PDF' tooltip".
