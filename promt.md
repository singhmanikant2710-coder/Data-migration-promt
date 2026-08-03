READ-ONLY. Diagnostics only. Do not change anything.

The footer in CroProductionSummaryPDF.tsx was changed to render 
`${title} • Page X of Y` (verified in diff), but it still appears blank/missing 
in the generated PDF. The footer TEXT may be rendering with an empty `title`.

Check the `title` value specifically in the FIRST footer (inside 
CroProductionSummaryPage):

1. Show the exact function signature / props destructuring of 
   CroProductionSummaryPage. Is `title` listed in the destructured props 
   parameter? Copy the exact signature line.

2. Find where CroProductionSummaryPage is rendered (the JSX call site, 
   likely inside the default export CroProductionSummaryPDF). Show that 
   exact line — is `title={...}` being passed, and what value/variable is 
   passed?

3. In the default export, show the line that defines `title` 
   (e.g. const title = props?.meta?.title || "CRO Review Production";) and 
   confirm what `props` is and whether props.meta exists.

4. MOST IMPORTANT: trace whether the `title` referenced INSIDE 
   CroProductionSummaryPage's footer is:
   (a) the destructured prop `title`, and 
   (b) whether that prop is actually passed a non-empty value at the call site.
   If title is NOT in the destructured params but the footer uses `title`, 
   it would be undefined → blank footer.

5. Also confirm: is there any OTHER `title` variable shadowing in scope? 
   And does the second footer (in the default export) use the same title?

Do not edit anything. Show the actual current signature, call site, and 
title definition. Findings only.
