FIX DIRECTLY — no options, no investigation report. Do not commit.

Real cause (confirmed by pattern): the Risk Rating Justification block jumps to the next page whenever its total content is taller than the space left on the page — even with only tiny images. Before the image fix the content just fitted. So the CONTAINER in ReviewPDF.tsx that holds the rich-text field is unbreakable, not HtmlRichText.

1. In ReviewPDF.tsx, find every View/Text that wraps a rich-text field (Risk Rating Justification and all other long-text sections using HtmlCell/HtmlRichText/longTextContainer/styles.section). Report each one that has wrap={false}, a fixed height, or is otherwise non-splittable (file:line).
2. Make those long-text containers breakable (remove wrap={false} / fixed height on the content container only). Keep the heading together with the start of its content by giving the heading minPresenceAhead (≈ 40pt) — do not keep the whole section together.
3. Keep wrap={false} on small fixed blocks (header bars, label/value rows, table header/data rows) exactly as today.
4. Apply the same check to InitialMemoPDF.tsx and FinalMemoPDF.tsx long-text sections, same rule.
5. Do not change section `break` props, page setup, or other reports.

Verify with real PDFs (page-mapping harness):
- test-data/rrj-20120.html in the Risk Rating Justification slot: text starts directly under the heading, flows to the next page, no blank half page, no blank pages, both tables + both images present.
- Same field with images removed but 2 pages of text: flows, no jump.
- Short fields: identical to today.

Report only: containers changed (file:line, before/after props), page placement before vs after for the cases above, tsc + npm run build + test:unit, git status. Do not commit or push.
