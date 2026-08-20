Apply the edits to HtmlRichText.tsx now, with ONE correction:

Your helper-insertion block ended with:
   function getTextAlignStyle(node: AstNode): any {
     return out;
   }
That "return out;" is wrong and would corrupt getTextAlignStyle. 

Do this:
1. First view the current getTextAlignStyle to see its REAL body.
2. Insert parseLen and readInlineStyleProp as standalone functions ABOVE
   getTextAlignStyle, leaving getTextAlignStyle's body completely unchanged.
3. Apply the other edits exactly as proposed: styles.table (add
   alignSelf:"flex-start" + width:"auto"), styles.image (remove height:120),
   the per-cell width logic in renderTableAst, and the renderImageAst rewrite.
4. After applying, show me the full body of getTextAlignStyle (to prove it's
   intact) and confirm no other function was changed.

Auto-approve OFF. Apply the changes, don't just re-print diffs.
