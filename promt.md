Single-file edit: frontend/src/components/pdf/CrmFindingsObservationsPDF.tsx

Fix: Rename the filter payload heading from "Current Filter Payload" to "Applied Report Filters". This string appears in BOTH the items-present branch and the no-items branch. Update BOTH.

BEFORE (both occurrences):
<Text style={styles.sectionTitle}>Current Filter Payload</Text>

AFTER (both occurrences):
<Text style={styles.sectionTitle}>Applied Report Filters</Text>

CONSTRAINTS:
- ONLY change the heading text, in both places it appears.
- Do NOT change buildFilterParagraph, styles, footers, or Page structure.
- Do NOT touch any other file.
- Only edit this one file. Show the diff.

- READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/components/pdf/CrmFindingsObservationsPDF.tsx

Show me ONLY the ConsolidatedFindingsObservationsPage function/component in full:
1. Its return — the <Page> open/close, what renders inside (the tables/sections), and the LAST element before </Page>.
2. Its footer handling (is there a fixed footer inside this Page?).
3. Whether it renders a SINGLE <Page> that auto-flows, or maps multiple <Page>s.
4. What props it receives (items, title, generatedOn, filters?) — does it already get `filters`?

Read once. Findings only. No edits.
