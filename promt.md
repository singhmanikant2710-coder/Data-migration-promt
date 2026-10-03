FIX DIRECTLY — no investigation report, no options. Do not commit.

Cause: the blank page after the "Risk Rating Justification" heading appeared only after follow-up 2. The images in the real field are tiny (148x41 and 77x30 px), so image size is not the cause. The new wrapper that renderInlineWithImages (and the li/h1/h2/blockquote/span branches from follow-up 2) puts around text+image segments is a View that react-pdf cannot split across pages, so the whole field jumps to the next page.

Fix:
1. renderInlineWithImages must NOT wrap its segments in a container View. Return a flat array (or React.Fragment) of siblings: each text run as the same <Text> the code produced before follow-up 2, each image as a bare <Image> (sized by computeImageLayout). The siblings are placed directly in the existing parent, so the parent's layout and page-splitting are exactly as before follow-up 2.
2. Same for every branch follow-up 2 changed (p/div flushInline, h1, h2, blockquote, li, strong/b/em/i/u/a/span at block level): no new wrapping View; no wrap={false}, minPresenceAhead or fixed height on text or on the image. For li, keep the bullet row; place images after it as siblings.
3. Keep the IMAGE_PAGE_SLACK / height cap from follow-up 3.
4. Content without images must be byte-identical to before follow-up 2.

Verify with real PDFs (your page-mapping harness), including test-data/rrj-20120.html if it exists:
- text starts directly under the heading, no blank half page, no blank pages
- both tables and both images render, images at 111x30.75pt and 57.75x22.5pt, in document order
- your previous 15 checks still pass

Report only: ADDED / CHANGED / REMOVED (file:line), page placement for rrj-20120.html before vs after, tsc + npm run build + test:unit, git status. Do not commit or push.
