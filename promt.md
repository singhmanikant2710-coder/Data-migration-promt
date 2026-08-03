Single-file edit: frontend/src/components/pdf/CroProductionSummaryPDF.tsx

PROBLEM: The footer container renders (its border-top line is visible at the 
bottom of pages), but the footer TEXT does not appear. The <Text> element 
inside the footer is malformed and not rendering.

FIX: Find BOTH occurrences of the footer (the <View style={styles.footer} fixed> 
blocks — one inside CroProductionSummaryPage, one inside the second Page of 
the default export CroProductionSummaryPDF). Replace each ENTIRE footer 
<View> block with this EXACT, well-formed code:

  <View style={styles.footer} fixed>
    <Text
      style={{ fontSize: 8, color: "#334155", textAlign: "center" }}
      render={({ pageNumber, totalPages }) => `${title} • Page ${pageNumber} of ${totalPages}`}
    />
  </View>

Requirements:
- The <Text> must be a properly self-closed element: it has exactly two 
  props (style and render), and ends with a self-closing "/>". 
- The render prop must be an arrow function returning a template literal 
  using backticks: `${title} • Page ${pageNumber} of ${totalPages}`.
- Do NOT wrap render in extra braces incorrectly. The correct form is:
  render={({ pageNumber, totalPages }) => `...`}
- Remove any existing malformed content like `{textRender={...}}` or stray 
  inner <View> wrappers or leftover fragments inside the footer.
- `title` is already in scope in both locations (resolves to 
  "CRO Review Production").
- Keep <View style={styles.footer} fixed> exactly as the outer wrapper 
  (its border-top provides the divider line).

CONSTRAINTS:
- Do NOT touch pageSetup.ts, page size, orientation, margins, styles.page, 
  styles.footer definition, header, or tables.
- Only replace the two footer <View> blocks so the <Text> is well-formed 
  and renders.
- After editing, print BOTH footer blocks VERBATIM exactly as they now exist 
  in the file, so I can verify the <Text> is syntactically correct.
