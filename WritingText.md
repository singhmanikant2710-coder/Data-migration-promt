Hi Geoff,

I've reviewed how the Initial Memo, Final Memo and CAS Linesheet handle comment formatting. In short:
- All three already use the same formatting engine, but 11 Linesheet fields go through an extra step that merges paragraphs and drops bold/italic for text pasted from Word. That is why the same comment can look fine in the memo but wrong in the Linesheet.
- Pressing Enter once and pressing Enter twice currently look almost the same in the PDFs, because blank lines are dropped.
- Text pasted from Word, PDFs or web pages keeps its hidden formatting (fonts, sizes, spacing), which the PDFs handle inconsistently.

My proposed approach:
1. First, apply one set of formatting rules to every comment field in all three PDFs: one Enter = next line with tight spacing, two Enters = a blank line between paragraphs; one font and size (11pt for narrative sections, 10pt for comments inside tables); bold, italic, underline, lists, tables and pictures kept. This fixes existing comments as well as new ones, without changing anything that is stored.
2. Then, clean up pasted text as it is pasted into the Review Form (keep bold/italic/lists/tables/pictures, remove fonts, sizes and hidden Word formatting), so less clean-up is needed in future.

A few points I'd like to confirm in a short demo (my suggestion in brackets):
1. One Enter: no extra space between lines? (Yes – this will make single-Enter text slightly tighter than today.)
2. Three or more Enters in a row: keep as extra blank lines, or reduce to one blank line? (Reduce to one.)
3. Word's normal paragraph spacing on pasted text: show it as a blank line, or tight like one Enter? (Blank line, so pasted paragraphs stay separated.)
4. Colours, highlights and centred/justified text: keep them, or show all comments in plain black, left-aligned? (Keep what users add with the toolbar; drop colours that come in with a paste.)
5. Text pasted from a PDF often breaks at the end of every line: keep those line breaks, or try to join them into paragraphs? (Keep them – joining is a guess and can be wrong.)
6. Pasted tables and pictures: keep as they are today? (Yes.)
7. Memos that are already finalized: should they show the new spacing if regenerated, or only new/in-progress reviews? (All – the PDFs are generated on demand.)
8. Empty Scorecard comments: hide the section (as the memos do) or show "-" (as the Linesheet does)? (Hide.)
9. Font sizes: 11pt for narrative sections and 10pt for comments in tables, in all three PDFs? (Yes.)

For the demo, it would help if you could share 3–4 review IDs where the formatting looks wrong (e.g. pasted from Word, from a PDF, and typed with blank lines). I'll show before and after on those reviews.

Please let me know a time that works for you.

Thanks,
Manikant

#262 (Memo / Linesheet formatting) – Analysis complete. Cause identified: an extra formatting step on 11 Linesheet comment fields (merges pasted paragraphs and drops bold/italic), blank lines dropped in all three PDFs, and no clean-up of pasted text. Proposed approach and questions sent; demo requested to confirm the spacing rules before development.
