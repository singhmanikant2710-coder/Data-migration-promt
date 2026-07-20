Read-only. No edits. No plan. Just report with exact code. Do NOT modify or revert anyone's existing work (including Jothi's).

File: frontend/src/components/ui/RichTextEditor.tsx

Goal: add toolbar actions to edit a table AFTER it has been inserted — add row, delete row, add column, delete column. Column resize is NOT required.

Report:
1) Paste the existing selection/caret tracking code: saveCurrentSelection, restoreSelection, lastRangeRef, caretIndexRef, focusEditor, and getCaretCharacterOffsetWithin. Show how the current selection is stored and restored around toolbar actions.

2) Paste the ToolbarButton component definition and one example of how a toolbar button is wired (e.g. the Table button), so new buttons can follow the same pattern.

3) Paste insertTable() and insertHtmlAtSelection() in full (already partly seen) plus handleInput() — I need to know exactly what must be called after a DOM mutation so the editor's value/state and onChange are updated correctly.

4) Confirm: after a direct DOM mutation inside editorRef.current (e.g. appending a <tr>), what is the correct sequence to (a) persist the change to component state/onChange, and (b) keep the caret in a sensible place? Show the existing pattern used elsewhere in this file.

5) Is there any existing helper that, given the current selection, finds the nearest ancestor element of a given tag (e.g. closest <td> / <table>)? If not, say so.

Output only the code and answers. Change nothing.
