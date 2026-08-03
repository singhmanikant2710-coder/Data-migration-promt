Single-file edit (DIAGNOSTIC): frontend/src/components/pdf/CroProductionSummaryPDF.tsx

The render prop does not fire in the CRO footer (yellow bg shows, but no 
text even with a literal string in render). Testing whether <Text> renders 
static children at all in this footer context.

In the FIRST footer ONLY, use STATIC children text (NO render prop):

  <View style={[styles.footer, { backgroundColor: "#FFFF00" }]} fixed>
    <Text style={{ fontSize: 12, color: "#FF0000", textAlign: "center" }}>
      STATIC FOOTER TEXT WORKS
    </Text>
  </View>

Generate/save.
- If "STATIC FOOTER TEXT WORKS" (red) appears → <Text> renders fine; ONLY 
  the `render` prop is broken in this context. We'll switch to a different 
  page-number approach.
- If still nothing (only yellow) → <Text> itself doesn't render inside this 
  fixed footer in the CRO document structure — a deeper Page/Document 
  structural problem.

ALSO — this is important — show me the EXACT top-level structure of how CRO 
renders its Document and Pages vs how PD Grade does:
1. In CroProductionSummaryPDF (default export): show the full <Document> ... 
   </Document> JSX skeleton — how <CroProductionSummaryPage /> is placed, and 
   the second <Page>. Is CroProductionSummaryPage returning a <Page> itself, 
   or a <View>? Show the return statement opening of CroProductionSummaryPage 
   (does it return <Page ...> or <View ...> or a Fragment?).
2. In CrmPdGradeMigrationPDF (working): show the same — does 
   CrmPdGradeMigrationPage return a <Page>? 
3. KEY: confirm whether CroProductionSummaryPage returns a <Page> as its 
   ROOT element, or whether it returns something else that is then wrapped 
   in a <Page> at the call site. The `fixed` + `render` page context only 
   works when the footer's <View fixed> is a direct child of a <Page>.

Show the changed footer block, the static-text result expectation, and the 
Document/Page structure comparison.
