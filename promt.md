Apply the 4-file font fix, but File 4 (MemoPdfModal.tsx) has issues in your diff.
Fix these before applying File 4:

1. Rules of Hooks violation: your new useEffect is placed AFTER `if (!open) return
   null;`. React hooks must be at the top of the component, before any early
   return. Move the font-warming useEffect UP next to the existing data-fetch
   useEffect, before `if (!open) return null;`.

2. Consistency: the file imports hooks as named imports (useState, useEffect) —
   do NOT use React.useRef / React.useEffect unless React is imported. Either add
   `import { useRef } from "react"` and use useRef/useEffect directly (matching
   existing style), or ensure React is imported. Match whatever the file already
   does.

3. Do NOT add duplicate imports. If initialMemo/finalMemo/types are already
   imported for the data fetch, don't re-import them. Only add
   `import { ensureFontsRegistered } from "@/components/pdf/pageSetup"` and
   whatever hook (useRef) is genuinely new.

Apply Files 1, 2, 3 as shown (they're correct). For File 4, re-do it with the
hook at the top and correct imports. Then show me the corrected File 4 diff and
confirm the useEffect is before the early return. Auto-approve OFF.
