READ-ONLY. Diagnostics only. Do not change anything.

In CroProductionSummaryPDF.tsx, the footer renders 
`${title} • Page X of Y` but appears BLANK/empty in the generated PDF. 
I need to find why `title` is empty.

Show me:
1. In CroProductionSummaryPage: how is `title` received? Show the component 
   signature/props destructuring. Is `title` a prop, and does it have a 
   default/fallback if not passed?

2. Where is CroProductionSummaryPage rendered/called (in the default export 
   CroProductionSummaryPDF or elsewhere)? Show that call site — is the 
   `title` prop actually being PASSED to CroProductionSummaryPage? If not, 
   `title` would be undefined and the footer would show " • Page X of Y".

3. In the default export, confirm the `title` variable: 
   const title = props?.meta?.title || "CRO Review Production"; 
   — is this in scope for BOTH footer occurrences, or only the second Page? 
   The first footer (inside CroProductionSummaryPage) may be using a 
   different/undefined `title`.

4. Show what value `title` resolves to in the CroProductionSummaryPage 
   footer specifically.

Do not edit anything. Findings only.
