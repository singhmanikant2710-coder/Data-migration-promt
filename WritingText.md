UAT #257 – Remove HTML/rich-text markup from long-text fields in Report Excel exports

REQUIREMENT (Geoff)
Excel exports from Reports Home must not contain HTML/rich-text "jargon" (tags, &nbsp;, &amp;, style attributes, etc.) in comment fields:
- CRM Summary: Risk Rating Justification (column R) and Finding Comments (column AC).
- CRM Findings & Observations: Comments.
- Also confirm the six #256 exports (CRM Summary for Management, CRM Findings for Management, Scorecard Results, Policy Exceptions, Non-Compliant Covenants, CRO Production) are free of it.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- onExport branches in reports/page.tsx for CRM Summary and CRM Findings & Observations (file:line): which fields are written raw.
- How the #256 builder converts rich text (lib/reportExcelExport.ts – stripHtmlToText / decodeEntities, file:line): does it keep paragraph/line breaks and list bullets, and handle tables/images in rich text?
- Every other remaining Excel export (CRM Sample Summary, CRM Final Memos, Covenants Summary, Review Queue #237): list any rich-text fields still written raw.

PHASE 2/3 – PLAN + IMPLEMENT (smallest generic change)
- Reuse ONE shared plain-text helper (the #256 one) for every rich-text cell in every Excel export – no second copy.
- Output: readable plain text – paragraphs and <br> become line breaks, list items become "• " lines, entities decoded (&amp; → &, &nbsp; → space), all tags/styles removed, extra blank lines collapsed, cell set to wrap text. Plain (non-HTML) text unchanged. NULL → empty cell.
- Keep the #237 formula-injection guard and the 32,767-character limit.
- Excel only – PDFs unchanged.

PHASE 4 – VERIFY + REPORT (in chat, not in a file)
- ADDED / CHANGED / REMOVED (file:line).
- Before/after examples: a comment with paragraphs, bold, a bullet list, &amp;/&nbsp;, a table, an image, plain text, NULL.
- Confirm each export (CRM Summary, CRM Findings & Observations, the six #256 exports, other remaining exports) – which fields were cleaned.
- npx tsc --noEmit, npm run build.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo if rewritten; leave pre-existing SQL scripts and devin-output/ alone).
- Do not commit or push.
