READ-ONLY. Do NOT edit. Investigate and report only.

Two bugs to fix in the memo PDF rich-text rendering (HtmlRichText.tsx), for
content pasted from Word/Excel:

#193 — Pasted tables stretch to the full page width instead of sizing to content.
#194 — Pasted images enlarge / lose their original size.

Report the current state so I can write a bounded fix:

1. In HtmlRichText.tsx, show the current table styles (styles.table, tableRow,
   tableCell) and renderTableAst(). Confirm: does styles.table have any width
   constraint? Do cells use flexGrow:1 / flexBasis:0 (equal-width, fill-parent)?
   Is any HTML width attribute or inline CSS width read at all?

2. Show the current image handling: styles.image and renderImageAst(). Confirm
   the fixed height (e.g. height:120), maxWidth, and whether any width/height
   from the <img> attributes or inline CSS is read.

3. Confirm these are the ONLY places tables and images are rendered in this file,
   and that changing them won't affect the memo Page layout elsewhere.

4. Note whether a shared style object is used that other components also rely on
   (so I don't change something with wider impact).

Output: current table code + current image code + confirmation of scope. No edits.
