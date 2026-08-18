Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Fix: Section headings (sectionTitle) can get orphaned at the bottom of a page while their table flows to the next page. This happens depending on data size (some samples break, some don't). Apply a data-independent orphan control: add minPresenceAhead to each section heading so the heading only renders if there's enough room below for the start of its table; otherwise it moves to the next page WITH its table.

This is safer than wrap={false} on the whole section (which would be unsafe for the potentially large Subreport01/02 tables). minPresenceAhead on just the heading keeps the heading attached to its table's start without forcing the whole table together.

Apply to the sectionTitle heading in ALL these sections: Subreport01_Count, Subreport02_Commitment, Subreport03_DistByCount, Subreport04_DistByExposure, MatrixCount, MatrixCount, MatrixCommitment.

For each, change the heading from:
<Text style={styles.sectionTitle}>...heading text...</Text>
to:
<Text style={styles.sectionTitle} minPresenceAhead={40}>...heading text...</Text>

(Use minPresenceAhead={40} — enough to guarantee the heading isn't left alone at page bottom; the table header row already has its own minPresenceAhead: 28, so 40 on the heading keeps heading + table-header together.)

CONSTRAINTS:
- ONLY add minPresenceAhead={40} to the sectionTitle <Text> headings in the listed sections.
- Do NOT change the sectionTitle style definition itself (that would affect ALL uses including "Applied Report Filters" and "Detail" — we only want it on these section headings).
- Do NOT change wrap props, table structure, rows, or minPresenceAhead on existing trHeader/trTotalsReserve.
- Do NOT touch fonts, alignment, or any prior fix.
- Do NOT touch any other file.
- Show the FULL diff so I can confirm it's applied per-heading (not to the shared style).
