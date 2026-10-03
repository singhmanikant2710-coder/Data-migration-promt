Follow-up 3 for UAT #194 (same task, do not commit):

Regression after follow-up 2: in the CAS Linesheet PDF, the "Risk Rating Justification" heading stays at the top of page 2, the rest of that page is blank, and the WHOLE field content (paragraphs + Excel table + Word table + images) starts on page 3. Before follow-up 2, the text started directly under the heading and flowed across pages. The field contains multiple <p class="MsoNormal"> paragraphs, two tables and two data-URI images, wrapped by ReviewPDF in <div style="font-size:11px">.

Phase 1 (read-only): with file:line, show the element tree now produced for this field (View/Text/Image nesting, any wrap={false}, minPresenceAhead, fixed heights) vs before follow-up 2, and identify what made the content unbreakable or forced it to the next page (e.g. renderInlineWithImages wrapping all segments in one View, wrap={false}, or one giant Text). Also check the ReviewPDF field container and heading for wrap/minPresenceAhead settings.

Fix (generic, smallest change):
- Long rich-text content must flow across pages exactly as before: text segments breakable, no wrapping View that is wrap={false}, no single block forced to the next page.
- Only an individual Image (and a table row, as today) may be unbreakable.
- The heading should stay with at least the first lines of its content (keep existing behaviour if that already exists; don't add new rules otherwise).
- Fields WITHOUT images must render byte-identically to before follow-up 2 (and to before this task).
- Keep the image-in-<p> fix working (image in document order, no extra blank lines).

Verify on the real rendered component AND by rendering a PDF to check page placement: long field (2+ pages of text) with an image in the middle; long field with no image; short field with image near a page end; image taller than remaining page space (only the image should move, text before it stays).

Report:
1. Root cause (file:line)
2. ADDED / CHANGED / REMOVED lines (file:line)
3. Before vs after page placement for each case above
4. Fields without images: confirm identical output
5. Build results (tsc + npm run build + test:unit)
6. git status (htmlLayout.ts is our new file, not an artifact)
Do not commit or push.
