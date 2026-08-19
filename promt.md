Single-file edit: frontend/src/components/ui/SearchableSelect.tsx

Fix (Bug #188): The dropdown menu always opens downward (openUp hardcoded false) and its top is only clamped, so when the trigger is near the bottom of the viewport the 320px menu runs off the page and options get cut off. Make it flip UP only when there isn't enough room below — otherwise keep the exact current downward behavior (backward compatible for all other usages).

This is a shared component; the change must NOT alter behavior in the common case (menu opens down). Only when space below is insufficient AND there's more room above should it open up.

In updateMenuPos, replace the hardcoded openUp + downward-only top calc:

BEFORE:
  const rect = (btn as HTMLElement).getBoundingClientRect();
  const openUp = false;
  ...
  // Vertical position: below trigger (we still flip logic off for now)
  const top = Math.min(
    window.innerHeight - menuMargin,
    rect.bottom + menuMargin
  );

  setMenuPos({
    top,
    left,
    width: desiredWidth,
    openUp
  });

AFTER:
  const rect = (btn as HTMLElement).getBoundingClientRect();

  // Space available below and above the trigger
  const spaceBelow = window.innerHeight - rect.bottom - menuMargin;
  const spaceAbove = rect.top - menuMargin;

  // Only flip up if there isn't enough room below for the menu AND there's more room above.
  // Otherwise keep the original downward behavior unchanged.
  const openUp = spaceBelow < menuMaxHeight && spaceAbove > spaceBelow;

  let top: number;
  if (openUp) {
    // Position the menu so its bottom sits just above the trigger.
    // Cap its height to the space above so it never runs off the top.
    const menuHeight = Math.min(menuMaxHeight, spaceAbove);
    top = Math.max(menuMargin, rect.top - menuMargin - menuHeight);
  } else {
    // Original downward behavior (unchanged).
    top = Math.min(window.innerHeight - menuMargin, rect.bottom + menuMargin);
  }

  setMenuPos({
    top,
    left,
    width: desiredWidth,
    openUp
  });

CONSTRAINTS:
- Do NOT change the width/left/horizontal logic — only the vertical (top) + openUp computation.
- Downward case must remain EXACTLY as before (same top formula) so existing usages are unaffected.
- Do NOT change menuMaxHeight, the portal div, or the menu container styles.
- openUp is already in state; just compute it instead of hardcoding false.
- Do NOT touch any other file.
- Show the diff.
