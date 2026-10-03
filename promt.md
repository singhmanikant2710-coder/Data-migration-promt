Follow-up 4 for UAT #194 — FIX DIRECTLY, do not stop for options. Do not commit.

Follow-up 3 did NOT fix the real case. Review 20120, CAS Linesheet PDF: "Risk Rating Justification" heading, then the rest of the page is blank, and the whole field content starts on the next page. Both images in this field are SMALL (148x41 px and 77x30 px) — the page-tall-image theory does not apply.

The EXACT live field HTML (full base64 images) is at: test-data/rrj-20120.html
Use ONLY this file as the reproduction. Do not modify or delete it.

Do this in one pass:
1. Reproduce: render this exact HTML through HtmlRichText inside the same ReviewPDF containers used for Risk Rating Justification (same section/longTextContainer styles, availableWidth/availableHeight, and a preceding section with `break` so the heading starts near the top of a page). Confirm the blank space with your page-mapping harness. Also render the same HTML with both <img> removed, and on the HEAD versions of HtmlRichText.tsx/htmlLayout.ts.
2. Find the exact element(s) that cannot split (View with wrap={false}, a View that wraps the whole field, a single giant Text, a table, minPresenceAhead, fixed height, etc.) — file:line.
3. Fix with the smallest generic change: rich-text content must start directly under its heading and flow across pages; only an individual Image and a table row may be unbreakable. Do not change ReviewPDF section breaks or other reports.
4. Acceptance (must ALL pass on test-data/rrj-20120.html, real PDF):
   - text begins on the same page as the heading, no blank half page
   - both images and both tables render, images at natural size (148x41 → 111x30.75pt, 77x30 → 57.75x22.5pt)
   - no blank pages anywhere
   - same HTML with images removed = identical to HEAD
   - your previous 15 checks still pass (no-image fields byte-identical, image order, no extra blank lines, page-tall image case)

Report (short):
1. Root cause (file:line) and why follow-up 3's harness missed it
2. ADDED / CHANGED / REMOVED lines (file:line)
3. Per-page placement for rrj-20120.html: HEAD vs before-fix vs after-fix
4. Build: tsc + npm run build + test:unit
5. git status (test-data/ and htmlLayout.ts noted correctly)
Do not commit or push.
