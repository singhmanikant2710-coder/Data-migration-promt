The dropdown is now too narrow to be readable — descriptions don't show at all because the menu width is tied to the trigger (column) width, which is small.

Fix in the SearchableSelect portal menu (and its usage in CrmFindingsAndRatingsSection.tsx):

- The MENU must NOT inherit the trigger width. Give it a fixed, generous width: min-width 640px, max-width 900px (and max-width: 90vw so it never overflows the viewport). It should be left-aligned to the trigger but allowed to extend to the right beyond the column.
- Keep max-height ~320px with vertical scroll.
- Remove any horizontal scrollbar inside the menu — the description must WRAP instead (whitespace-normal, break-words).
- Each option row: two columns — code in a fixed ~90px left column (nowrap, medium weight), description in the remaining space, wrapping to multiple lines. Row padding ~8px 12px, comfortable line-height, hover highlight.
- Search input at the top: full width of the menu.
- Closed trigger: still shows only the CODE (unchanged), and the trigger keeps its current small column width.
- The page must NOT gain horizontal scroll — the menu is an overlay/portal, so it should float above content without stretching the table.

Keep the option value = raw code and the save path unchanged. Show me the diff before applying. STOP if another file is needed.
