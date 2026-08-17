READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/components/pdf/CrmSummaryTablePDF.tsx

The rendered PDF still shows BOTH filter blocks. I need to see the exact current state. Show me ONLY (no edits):

1. The first-page inline filter block. Search for the block that renders "APPLIED REPORT FILTERS" followed by the long interpolated string starting "REPORT NAME: ...". Show the FULL block including its opening condition (is it `{filters ? (` ... `) : null}` ?) and closing. I need to see exactly how it's wrapped so it can be removed cleanly.

2. The last-page block. Search for "Current Filter Payload" (or "CURRENT FILTER PAYLOAD"). Show the exact <Text> heading line and the buildFilterParagraph line beneath it. Confirm the heading text as it currently stands.

3. Confirm: are these two blocks in the SAME return/Document, or in different branches (e.g. one in the main page, one in a separate <Page>)? Show enough surrounding structure (Page boundaries) to tell them apart.

Read once. Findings only. No edits.
