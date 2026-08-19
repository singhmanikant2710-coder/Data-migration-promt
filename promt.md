READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/components/ui/SearchableSelect.tsx

Show me ONLY the updateMenuPos function as it is NOW (the full function body). Confirm:
1. `const rect = ...getBoundingClientRect()` appears EXACTLY ONCE (no duplicate declaration).
2. `openUp` is computed (const openUp = spaceBelow < menuMaxHeight && spaceAbove > spaceBelow) — appears once, no leftover `const openUp = false`.
3. The if/else for `top` (openUp branch + downward branch) is present and syntactically correct.
4. setMenuPos is called once with { top, left, width, openUp }.

Read once. Findings only. No edits. I need to confirm no duplicate `rect` or `openUp` that would cause a build error.
