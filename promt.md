Frontend only. Create ONE new file. Do NOT modify any existing file in this step. Do not plan. Just create the component.

Create: frontend/src/components/pdf/HtmlRichText.tsx

Purpose: convert a safe subset of HTML (from our RichTextEditor) into @react-pdf/renderer components, so tables and images render in generated PDFs instead of being flattened to text.

Requirements:

- Props: { html?: string | null; fallback?: string }
- Import { View, Text, Image, StyleSheet } from "@react-pdf/renderer"
- Parse the HTML WITHOUT adding any new npm dependency. Use a dependency-free approach: since @react-pdf renders in the browser here, you may use DOMParser (new DOMParser().parseFromString(html, "text/html")) guarded with a typeof window check, falling back to plain stripped text if unavailable.
- If html is empty/blank after parsing, render <Text>{fallback ?? "-"}</Text>

Supported tags (ignore everything else, never throw):
  p, div, br            -> block paragraphs / line breaks
  strong, b             -> fontWeight 700
  em, i                 -> fontStyle italic
  u                     -> textDecoration underline
  h1, h2                -> larger bold block text
  ul, ol, li            -> bulleted / numbered list rows with indent
  table, thead, tbody, tr, th, td -> rebuilt as View rows/cells
  img                   -> <Image> using the src attribute
  a                     -> render its text content only

Table rendering (this is the key requirement — the client said borders are optional but ROW AND COLUMN SPACING is essential):
  - Table container: View { flexDirection: "column", borderWidth: 1, borderColor: "#cbd5e1", borderStyle: "solid", marginBottom: 6 }
  - Row: View { flexDirection: "row", borderBottomWidth: 1, borderBottomColor: "#cbd5e1", borderBottomStyle: "solid" } — last row no bottom border
  - Cell: View { flexGrow: 1, flexBasis: 0, minWidth: 0, padding: 4, borderRightWidth: 1, borderRightColor: "#cbd5e1", borderRightStyle: "solid" } — last cell in each row no right border
  - Cell content in <Text style={{ fontSize: 9 }}>
  - th cells: backgroundColor "#f1f5f9", fontWeight 700
  - Distribute cells evenly (equal flexBasis). Ignore colspan/rowspan for now — do not attempt to merge cells.

Image rendering:
  - <Image src={src} style={{ maxWidth: "100%", height: 120, objectFit: "contain", marginBottom: 4 }} />
  - Accept data: URLs and absolute http(s) URLs. If src is missing or the tag fails, render nothing rather than throwing.

Text styling defaults: fontSize 9, lineHeight 1.4, paragraph marginBottom 4, list indent 12.

Robustness: wrap the whole parse in try/catch. On ANY error, fall back to rendering the plain text with tags stripped (same behaviour as today), so the PDF never fails to generate.

Export as a default or named React component. Do not modify ReviewPDF.tsx, the memos, or anything else in this step.
