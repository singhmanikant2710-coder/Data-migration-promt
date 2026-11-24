UAT "262" (Memo / Linesheet PDF formatting) – DISCOVERY ONLY, no changes

REQUIREMENT (Geoff)
- Initial Memo, Final Memo and CAS Linesheet must apply the SAME formatting rules to comment fields, and every comment field inside the Linesheet must use the same rules.
- Paragraph spacing as typed in the Review Form should carry into the PDFs: one Enter = new line with tight spacing; two Enters = a blank line (paragraph spacing).
- Text pasted from Word, PDF or web pages currently produces inconsistent formatting in the Linesheet; users spend time cleaning it up.

PHASE 1 – READ-ONLY DISCOVERY
- List every comment/rich-text field in InitialMemoPDF.tsx, FinalMemoPDF.tsx and ReviewPDF.tsx (Linesheet) with file:line and HOW each is rendered (HtmlRichText, stripHtml, plain Text, custom). Show where the rules differ (paragraph margins, <br>, empty <p>, lists, fonts/sizes, hyphenation, table handling).
- How the Review Form editor (RichTextEditor.tsx) stores Enter vs Shift+Enter vs Enter-twice (exact HTML produced), and what pasted Word/PDF/web content looks like when stored (mso styles, spans with font-size, <o:p>, nested divs, inline styles).
- Is there any paste clean-up today in the editor?

PHASE 2 – PROPOSAL (no code)
- One shared rendering rule set for all comment fields in all three PDFs (what spacing an empty paragraph / <br> / list / table gets).
- Paste handling options: (A) clean pasted HTML on paste in the editor (keep bold/italic/lists/tables, drop fonts/sizes/colours/inline styles), (B) normalise at PDF render time only, (C) both. Pros/cons and impact on existing saved comments.
- Risks: existing reports that users have already formatted by hand; #241 page-break behaviour; Excel exports (#257) unaffected.
- Questions for Geoff to confirm in a demo.

STOP after Phase 2. Report in chat only.
