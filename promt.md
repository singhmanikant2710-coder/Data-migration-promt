Frontend only. Single file: frontend/src/components/ui/RichTextEditor.tsx
Do NOT modify any other file. Do NOT change any existing function, button, or behaviour — only ADD. This is a shared component used across many screens, so nothing existing may break. Do not plan. Just apply.

Goal (UAT #65): let users edit a table AFTER it has been inserted — add row, delete row, add column, delete column. Column resize is NOT required.

1) Add a helper (place it near the other helpers, above the component's return):

   const findAncestorTag = (node: Node | null, tag: string): HTMLElement | null => {
     const el = editorRef.current;
     let cur: Node | null = node;
     while (cur && cur !== el) {
       if (cur.nodeType === 1 && (cur as HTMLElement).tagName.toLowerCase() === tag) {
         return cur as HTMLElement;
       }
       cur = cur.parentNode;
     }
     return null;
   };

   const getCurrentCell = (): HTMLTableCellElement | null => {
     focusEditor();
     const sel = window.getSelection();
     if (!sel || sel.rangeCount === 0) return null;
     const node = sel.getRangeAt(0).startContainer;
     return (findAncestorTag(node, "td") ?? findAncestorTag(node, "th")) as HTMLTableCellElement | null;
   };

2) Add these four handlers, each following the SAME post-mutation pattern already used by execCmd: mutate the DOM, then call handleInput(), then setTimeout(() => restoreSelection(), 0). If getCurrentCell() returns null, show a brief alert("Place the cursor inside a table cell first.") and return without changing anything.

   const cellStyle = 'border:1px solid #cbd5e1;padding:4px;';

   addTableRow():
     - get current cell -> its parent <tr> -> clone the row structure: create a new <tr> with the same number of <td> cells as the current row, each with cellStyle and &nbsp;
     - insert the new row immediately AFTER the current row (row.parentNode.insertBefore(newRow, row.nextSibling))

   deleteTableRow():
     - get current cell -> its parent <tr> -> its <table>
     - if the table has only ONE row, remove the whole <table> instead
     - otherwise remove just that <tr>

   addTableColumn():
     - get current cell -> its cellIndex -> its <table>
     - for EVERY <tr> in that table, insert a new <td> (cellStyle, &nbsp;) immediately AFTER the cell at that same index. If a row is shorter than the index, append at the end.

   deleteTableColumn():
     - get current cell -> its cellIndex -> its <table>
     - if every row has only ONE cell, remove the whole <table> instead
     - otherwise remove the cell at that index from every <tr> that has one

3) Add four ToolbarButtons using the exact existing ToolbarButton pattern, placed immediately AFTER the existing "Table" button:

     <ToolbarButton onClick={addTableRow} label="+Row" ariaLabel="Add table row" />
     <ToolbarButton onClick={deleteTableRow} label="-Row" ariaLabel="Delete table row" />
     <ToolbarButton onClick={addTableColumn} label="+Col" ariaLabel="Add table column" />
     <ToolbarButton onClick={deleteTableColumn} label="-Col" ariaLabel="Delete table column" />

Do NOT touch insertTable, insertHtmlAtSelection, handleInput, execCmd, focusEditor, saveCurrentSelection, restoreSelection, or any existing toolbar button. Do NOT add any new dependency.

Run read-only TypeScript diagnostics on this file only.
