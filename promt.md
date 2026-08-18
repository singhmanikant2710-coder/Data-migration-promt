Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Fix: The minPresenceAhead={40} on the heading Text did not prevent orphaning (React-PDF 4.3.2 handles this more reliably via wrap={false} on a View). For the two BOUNDED distribution sections (Subreport03_DistByCount and Subreport04_DistByExposure, ~16 rows max), wrap the heading + table together by adding wrap={false} to their existing outer <View>, so the heading and its table always stay on the same page. Also remove the now-unnecessary minPresenceAhead from these two headings.

=== Subreport03_DistByCount ===
1. Add wrap={false} to the outer <View>:
BEFORE: <View>   (the outer view wrapping the "Final PD Distribution (Count)" heading + table)
AFTER:  <View wrap={false}>

2. Remove minPresenceAhead from its heading:
BEFORE: <Text style={styles.sectionTitle} minPresenceAhead={40}>Final PD Distribution (Count)</Text>
AFTER:  <Text style={styles.sectionTitle}>Final PD Distribution (Count)</Text>

=== Subreport04_DistByExposure ===
3. Add wrap={false} to the outer <View>:
BEFORE: <View>   (the outer view wrapping the "Final PD Distribution (Commitment)" heading + table)
AFTER:  <View wrap={false}>

4. Remove minPresenceAhead from its heading:
BEFORE: <Text style={styles.sectionTitle} minPresenceAhead={40}>Final PD Distribution (Commitment)</Text>
AFTER:  <Text style={styles.sectionTitle}>Final PD Distribution (Commitment)</Text>

CONSTRAINTS:
- ONLY add wrap={false} to the outer <View> of Subreport03_DistByCount and Subreport04_DistByExposure, and remove minPresenceAhead from those two headings.
- Do NOT change Subreport01/02 or the matrices (leave their minPresenceAhead as-is for now — those tables can be large, wrap={false} would be unsafe).
- Do NOT modify the sectionTitle style, table rows, or any other structure.
- Do NOT touch any other file.
- Show the FULL diff. Be careful to add wrap={false} to the CORRECT outer View for each subreport (match by the heading text inside).
