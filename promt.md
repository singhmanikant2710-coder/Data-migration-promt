Single-file edit (DIAGNOSTIC): frontend/src/components/pdf/CroProductionSummaryPDF.tsx

The footer structure now matches PD Grade exactly, but text still doesn't 
render. Testing whether the issue is the `title` prop variable inside the 
render callback.

In the FIRST footer ONLY, temporarily replace `title` with a LITERAL string 
and add a yellow background to confirm visibility:

  <View style={[styles.footer, { backgroundColor: "#FFFF00" }]} fixed>
    <Text
      style={{ fontSize: 12, color: "#FF0000", textAlign: "center" }}
      render={({ pageNumber, totalPages }) =>
        `CRO Review Production • Page ${pageNumber} of ${totalPages}`
      }
    />
  </View>

Note: this uses a HARDCODED literal string "CRO Review Production" instead of 
the ${title} variable — exactly like PD Grade's DetailTablePage footer which 
works with a literal.

Generate/save. 
- If "CRO Review Production • Page 1 of 5" (red) now appears → the problem 
  was the `title` prop (undefined at runtime inside render). 
- If still nothing (only yellow) → the render prop itself is not firing in 
  this component/page context, unrelated to title.

Show the changed footer block.
