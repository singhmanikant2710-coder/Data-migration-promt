Stop — your helper-insertion REPLACE (item 1) ends with:
   function getTextAlignStyle(node: AstNode): any {
     return out;
   }
That "return out;" is NOT the real body of getTextAlignStyle and would corrupt
that function. The helpers parseLen and readInlineStyleProp must be inserted
BEFORE getTextAlignStyle WITHOUT altering getTextAlignStyle's own body at all.

Fix: re-do only the helper insertion so that:
- parseLen and readInlineStyleProp are added as standalone functions, and
- getTextAlignStyle remains byte-for-byte unchanged (its original body intact).
View the current getTextAlignStyle first to confirm its real body, then insert
the helpers just above it. Then apply items 2–5 as already shown (those look
correct). After applying, show me getTextAlignStyle's full body to prove it was
not modified. Auto-approve OFF.
