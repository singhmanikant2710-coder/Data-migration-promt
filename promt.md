Yes — do the instrumentation, ONE round only, then fix directly in the same round. Do not commit.

1. Temporarily add console.log in node_modules/@react-pdf/layout splitNodes/shouldBreak for the RRJ subtree (height, box.top, box.height, shouldSplit, canWrap, slicedLineBreak). Run once on test-data/rrj-20120.html in the real ReviewPDF. Revert node_modules immediately after (verify clean).

2. Fix in HtmlRichText.tsx (generic): never emit one giant Text for a long run. Split a run into separate sibling <Text> elements at paragraph boundaries (<br><br>, block-level boundaries, and every single <br> if needed), with identical styles, so react-pdf can place whole paragraphs on the current page even when it cannot split a single Text. Visual output (line spacing, blank lines) must look the same as today; exact element-tree identity for no-image fields is NOT required for this change, visual identity is.

3. Acceptance on the real ReviewPDF with test-data/rrj-20120.html: RRJ text starts on page 2 directly under the heading, no half-empty page, both tables + both images present, no blank pages. Also check: same field without images; prose x2; a short field; Initial/Final Memo RRJ.

Report only: geometry found (1-3 lines), change (file:line), page placement before/after for each case, builds, git status. Do not commit or push.
