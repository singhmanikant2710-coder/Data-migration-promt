READ-ONLY. Diagnostics only. Do NOT change anything.

File: @/components/ui/SearchableSelect (the SearchableSelect component)

For the flip-up fix, show me ONLY (no edits):
1. The FULL position-calculation code block — where it computes menuPos (top, left, width, openUp). Show the getBoundingClientRect() usage, the `const openUp = false` line, the top clamping, and setMenuPos. I need the exact current logic to add flip-up.
2. The menuMaxHeight variable — its default value (320) and where it's defined/used.
3. Where menuPos.top is applied to the portal div (the style={{ top: menuPos.top, ... }}).
4. Is openUp used anywhere else in the render (e.g. to adjust styling/margin when opening up)? Show any openUp references.
5. Confirm: does the component measure the menu's actual height, or use a fixed menuMaxHeight? (This affects how I calculate whether to flip up.)

Read once. Findings only. No edits.
