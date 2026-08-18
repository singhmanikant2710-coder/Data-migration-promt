READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

For Item 4 (rename "Current Filter Payload" -> "Applied Report Filters" + move it directly below the details table). Show me ONLY (no edits):

1. The has-items branch of CrmPdGradeMigrationDocument — the full Document return. Show every <Page>: the summary page, DetailTablePages, and the separate filter Page. Show how DetailTablePages is rendered and where the filter Page sits relative to it.

2. The DetailTablePages / DetailTablePage components — how they chunk (22 rows/page), and what's the LAST content inside the last detail page before its footer. Do these receive `filters`?

3. Both "Current Filter Payload" blocks (has-items + no-items) with surrounding JSX + footer.

4. Since details use manual chunking (separate Page per chunk), tell me: can I append the filter block to the LAST detail page's content (so it appears right after the details), or is it cleaner to keep the filter Page but just rename it? Show enough structure to judge.

Read once. Findings only. No edits.
