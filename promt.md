Frontend only. Single file: frontend/src/components/pdf/HtmlRichText.tsx
Do NOT modify ReviewPDF.tsx or any other file. Do not plan. Just apply.

ROOT CAUSE (confirmed): the component uses DOMParser, but @react-pdf/renderer renders the tree via pdf().toBlob() in an environment where window/DOMParser are not available. The guard therefore always takes the plain-text fallback and the table/image branches never execute.

FIX: remove the DOMParser dependency entirely and replace it with a small dependency-free tokenizer/parser that works in ANY environment (no window, no DOM APIs, no new npm packages).

Requirements:

1) Delete every use of DOMParser, `document`, `Node.TEXT_NODE`, `Node.ELEMENT_NODE`, `HTMLElement`, `ChildNode`, `querySelectorAll`, `getElementsByTagName`, and any other DOM API. The component must not reference the DOM at all.

2) Write a minimal HTML parser using regex/string scanning that produces a plain JS AST:
     type Node =
       | { type: "text"; text: string }
       | { type: "element"; tag: string; attrs: Record<string,string>; children: Node[] };
   It must handle: nested tags, self-closing tags (<br/>, <img ... />), unquoted and quoted attribute values, and unclosed tags without throwing. Decode the common entities: &nbsp; &amp; &lt; &gt; &quot; &#39;.

3) Keep ALL the existing rendering behaviour and styles exactly as they are today (styles object, paragraph/h1/h2/list/table/cell/image styles, fontSize 9, borders #cbd5e1, header cell #f1f5f9, image style). Only the parsing layer changes — the render functions should now walk the new AST instead of DOM nodes.

4) Supported tags stay the same: p, div, br, strong, b, em, i, u, h1, h2, ul, ol, li, table, thead, tbody, tr, th, td, img, a, span. Ignore everything else but still render its children's text.

5) Table rendering must work: table -> View column, tr -> View row (last row no bottom border), th/td -> View cell with flexGrow 1 / flexBasis 0 / padding 4 (last cell no right border), th cells get the header background and bold. Cell content in <Text style={{fontSize:9}}>. Ignore colspan/rowspan.

6) Images: <Image src={src} /> only for src matching ^data:image/ or ^https?:// — otherwise render nothing. Never place Image inside Text.

7) Keep a final safety net: wrap the whole render in try/catch, and on error fall back to a regex-only plain-text strip (no DOM) rendered in a single <Text>.

The end result must render tables and images correctly inside pdf().toBlob() with no DOM available.

Run read-only TypeScript diagnostics on this file only.
