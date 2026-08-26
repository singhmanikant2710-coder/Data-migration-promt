READ-ONLY. No edits. Read frontend/src/blackbook/pdf/BlackBookPdf.tsx ONCE. Quote current state, stop.

CONTEXT: Client requires ALL months (up to 24: current fiscal year + prior fiscal year) to fit on a SINGLE PDF page, like the legacy report — NOT split across multiple pages. Currently caps are 6 and there may be chunking that creates multiple page-blocks. I need to see the page/layout setup to make it fit on one page.

Quote verbatim:

1) The <Page> element(s) and their size/orientation props. Is it size="LETTER" portrait, or landscape? Quote the Page opening tag(s) and any 'orientation' prop.

2) The styles object (StyleSheet.create) — specifically: styles.page (padding, size), styles.table, styles.tr, styles.td, styles.th, styles.sectionHeader. Quote font sizes, padding, and any height on rows. I need to know how tall each row is, to judge if 24 rows fit on one page.

3) The monthlyChunks rendering: does monthlyChunks.map(...) create multiple <View> sections on the SAME page, or could it create multiple <Page> elements? Quote the .map and confirm whether all chunks render within one <Page> or spill to new pages.

4) Is there a LEGAL_LANDSCAPE or landscape constant defined/imported anywhere (even if unused / in the disabled r24 block)? Quote it.

5) Whether any <View> has wrap={false} or a fixed height that would force content onto one page or prevent fitting.

OUTPUT:
- A) Page size/orientation, quoted.
- B) Row height / font size from styles, quoted — and your estimate: can ~24 compact rows fit on one page at this size? 
- C) Does the current render keep everything on one <Page>, or split? Quoted evidence.
- D) Any landscape constant available, quoted.
- State plainly what needs to change to fit all months on ONE page: (i) raise caps to 12, (ii) make monthlyChunks a single chunk (no multi-chunk), (iii) switch to landscape and/or reduce row font/padding. List which are needed based on the evidence.
- No fix yet. Findings only.
