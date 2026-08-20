Before applying: your SEARCH blocks for the helper insertion and for
renderImageAst look malformed (the helper SEARCH shows "return out;" inside
getTextAlignStyle, and renderImageAst's function signature appears duplicated
with no closing REPLACE marker). Do NOT corrupt existing functions.

Re-apply carefully:
1. View the actual current content of HtmlRichText.tsx around parseCssStyle,
   getTextAlignStyle, renderTableAst, renderImageAst, and styles.image FIRST.
2. Insert parseLen + readInlineStyleProp as NEW standalone helpers near the other
   helpers — do not overwrite or merge into getTextAlignStyle or any existing fn.
3. Apply the styles.table change (add alignSelf:"flex-start" + width:"auto"),
   styles.image change (remove height:120), the table per-cell width logic, and
   the renderImageAst rewrite — each as a clean, exact edit.
4. After editing, show me the final version of: parseLen, readInlineStyleProp,
   styles.table, styles.image, the cell-width snippet in renderTableAst, and the
   full renderImageAst — so I can verify nothing else changed and there are no
   duplicate functions.
Auto-approve OFF.
