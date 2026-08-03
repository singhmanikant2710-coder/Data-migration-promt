Single-file edit (DIAGNOSTIC): frontend/src/components/pdf/CroProductionSummaryPDF.tsx

The two CRO <Page>s have DIFFERENT footer code right now:
- CroProductionSummaryPage (page 1): diagnostic footer with nested <Text render> 
  (yellow bg, red). 
- The second payload <Page> (last page): already has a clean single <Text> 
  with DIRECT render: `${title} • Page ${pageNumber} of ${totalPages}` — 
  identical to PD Grade's working footer.

KEY QUESTION: On the SECOND page (the "Current Filter Payload" page), does the 
footer show "CRO Review Production • Page X of Y" WITH the page number, or 
not? That page already uses the exact PD-Grade pattern.

To test cleanly, make BOTH footers identical to PD Grade's EXACT working 
pattern (single Text, direct render, NO nested Text, NO diagnostic styling):

Replace BOTH CRO footers with EXACTLY this (matching PD Grade verbatim):

  <View style={styles.footer} fixed>
    <Text
      style={{ fontSize: 9, color: "#0F172A", textAlign: "center" }}
      render={({ pageNumber, totalPages }) =>
        `${title} • Page ${pageNumber} of ${totalPages}`
      }
    />
  </View>

(Note: use fontSize 9 and color #0F172A to match PD Grade exactly, in case 
styling matters. Remove ALL diagnostic styling, nested Text, and background.)

Then generate the PDF and check EACH page's footer:
- Does page 1 (summary) show the page number?
- Does the last page (payload) show the page number?
- Report which pages show "Page X of Y" and which show nothing or partial.

Show both final footer blocks verbatim.
