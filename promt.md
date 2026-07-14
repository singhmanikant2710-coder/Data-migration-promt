The description is now only in a tooltip (on hover). That is NOT what the client wants.

Client requirement: "Previous edit allowed user to see Finding Code Description ALONGSIDE Finding Code and all rows were even."

The description must be VISIBLE in each option row, next to the code — not hidden in a tooltip.

Fix the renderOption in the SearchableSelect usage for Finding Code:

Each option row must render TWO VISIBLE COLUMNS side by side:
- LEFT column: the code — fixed width ~90px, nowrap, medium weight.
- RIGHT column: the description — VISIBLE text filling the remaining width, truncated to a SINGLE line with ellipsis (CSS: overflow hidden, text-overflow ellipsis, white-space nowrap) so all rows have the SAME height.

So the list should look like:
  CS-101   Required borrower financial information past due and/or not obtai…
  CS-102   Required guarantor financial information past due and/or not obta…
  CS-103   Borrower/guarantor financial statements outside of policy quality …

For this to work the MENU must be WIDE enough:
- Give the portal menu a fixed width: min-width 640px, max-width 900px, and max-width 90vw. It must NOT inherit the trigger/column width.
- It is an overlay (portal), so it floats above the table and must not cause page horizontal scroll.
- Keep max-height ~320px with vertical scroll, and the search box at the top.

Keep the title attribute with the full description as a bonus tooltip, but the description MUST also be visible in the row.

Do NOT change renderSelected — the closed trigger correctly shows only the code, keep that.

Option value stays the raw code — save path unchanged.

Apply and show me the diff.
