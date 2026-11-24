UAT #261 (Bug) – CRM Summary for Management PDF: rich-text RRJ with pictures; remove "**" prefix in findings table

REQUIREMENT (Geoff)
1. RISK RATING JUSTIFICATION must render the rich text exactly as entered in the Review Form (bold, lists, tables, paragraph spacing) and include pasted pictures – the same way the standard CRM Summary report shows it.
2. CRM Findings table: keep the CRM Component column, but remove the "**" prefix from the component text, keeping the existing ascending order.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- ReviewSummaryForManagementPDF.tsx: how RRJ is rendered today (plain text / stripHtml?) and where the "**" comes from (data or PDF code), file:line.
- How the standard CRM Summary PDF renders RRJ with images (HtmlRichText.tsx, image handling, page breaks for tall content).
- Shared helpers exported for #256 Excel from these PDFs – must keep working.

PHASE 2/3 – PLAN + IMPLEMENT (smallest change)
- Reuse the same HtmlRichText rendering as CRM Summary for RRJ (no new renderer). Long RRJ / large images must break across pages without being cut off (see #241 lessons: no unbreakable oversized blocks).
- Remove only the "**" prefix in the display; sort order unchanged. If the prefix is used for sorting, keep sorting on the original value.
- Only this report changes; CRM Summary, other PDFs and the #256/#257 Excel exports unchanged.

PHASE 4 – VERIFY + REPORT (in chat, not in a file)
- ADDED / CHANGED (file:line), before/after for: RRJ with bold + bullets + table + pasted image; very long RRJ across pages; NULL/empty RRJ; findings table order and component text.
- npx tsc --noEmit, npm run build.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo; leave pre-existing SQL scripts and devin-output/ alone).
- Do not commit or push.
