READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/components/pdf/CrmFindingsObservationsPDF.tsx

Show me ONLY (no edits):
1. The END of ConsolidatedFindingsObservationsPage's return — from the closing of the sortedComponents map/content, through the new "Applied Report Filters" block, through the fixed footer, to the </Page> and function close. I need to see the exact JSX nesting/indentation to confirm the filter block sits INSIDE the Page (before footer) at the correct level, with no broken/mismatched tags.
2. The items-present branch Document return — confirm the separate filter Page is fully gone and filters={filters} is passed.
3. Confirm `filters` is defined in the scope of the function that renders the Document (show the line where filters is declared/destructured).

Read once. Findings only. No edits.
