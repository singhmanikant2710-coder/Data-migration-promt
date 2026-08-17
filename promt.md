READ-ONLY. Diagnostics only. Do not change anything.

File: frontend/src/app/load-samples/page.tsx (the Load Samples / "Select 
Sample" grid page)

Bug: When "Create Sample" is clicked, the grid enters inline edit-mode and 
renders a "(new)" row with input controls (date pickers, Select EIC dropdown, 
Type dropdown, Select Target dropdown). Two problems:
(A) In edit-mode the grid overflows horizontally — right-side columns (Type, 
    Target BU) and the Search button get cut off, and a horizontal scrollbar 
    appears inside the grid. Read-only/default view does NOT overflow.
(B) The page does not scroll vertically, so cut-off content can't be reached.

Show me (no edits):

1. GRID + EDIT ROW STRUCTURE: The component/JSX that renders the "Select 
   Sample" grid and the edit-mode "(new)" row. Show the row/cell structure for 
   BOTH read-only cells and edit-mode input controls (the date pickers, Select 
   EIC, Type, Select Target dropdowns), so I can compare their widths.

2. WIDTH/OVERFLOW CSS: The grid wrapper and the page container — what 
   width/height/overflow rules are applied (overflow-x, overflow-y, width, 
   max-width, fixed height, flex). Show the className/style on: the outermost 
   page container, the grid wrapper, and the scroll container (if any).

3. EDIT-MODE INPUT WIDTHS: The input controls in edit-mode — do they have 
   fixed width or min-width (e.g. the Select dropdowns had className="w-32", 
   date pickers, etc.) that make them WIDER than the read-only cells? List each 
   edit control's width class/style. This is likely why edit-mode overflows but 
   read-only doesn't — sum of fixed input widths exceeds the container.

4. VERTICAL SCROLL BLOCK: Which parent element blocks vertical scroll — look 
   for overflow: hidden, overflow-y: hidden, or a fixed height (h-screen, 
   fixed h-[...], max-h with overflow hidden) on the page container or a 
   wrapper. Show the element and the rule blocking vertical scroll.

Do NOT edit. Show: the grid/edit-row structure (read-only vs edit widths), the 
container/wrapper width/overflow CSS, each edit input's fixed/min width, and 
the element blocking vertical scroll. Findings only.
