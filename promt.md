READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/app/load-samples/page.tsx

Bug #188: The CRO Name dropdown doesn't show all its selections — it runs off the page (gets cut off) when there are more than 5 samples showing in the Select Sample window. Show me ONLY (no edits):

1. The CRO Name dropdown component/element — how it's rendered (is it a custom dropdown, a <select>, or a component like SearchableSelect?). Show the JSX and any wrapping container.
2. The dropdown's list/menu container styles — position (absolute/relative?), max-height, overflow, z-index. Show what controls how the dropdown list is displayed and whether it can scroll.
3. The parent container(s) of the sample grid rows — do any have overflow: hidden / overflow: auto that would clip the dropdown when it opens? Show the grid/row wrapper styles.
4. Is the CRO Name dropdown the same component used elsewhere (shared), or specific to this grid? If shared (e.g. SearchableSelect), note that so a fix here doesn't break other usages.

Read once. Findings only. No edits. I want to find why the dropdown is clipped/runs off page — likely a parent overflow:hidden clipping an absolutely-positioned dropdown, or the dropdown lacking max-height + scroll.
