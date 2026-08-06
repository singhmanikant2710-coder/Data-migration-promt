Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Keep the Totals row and footnote together with the matrix (prevent isolation 
on next page with whitespace), and fix footnote font. Both matrices.

PART A — Bind Totals + footnote together (Issues 3, 4, 5):
Currently the Totals row is in <View wrap={false}> but the footnote is a 
separate sibling <Text> after the table, not bound. So Totals can move to the 
next page and the footnote isolates with whitespace.

Wrap the Totals row AND the footnote together in a SINGLE <View wrap={false}> 
so they always stay as one unit:
    <View wrap={false}>
      {/* Totals row */}
      <View style={[styles.tr, styles.trLast]}> ...Totals... </View>
      {/* Footnote */}
      <Text style={{ ...footnote style... }}>CAS PD Rating totals represent...</Text>
    </View>

This keeps Totals + footnote together. If they don't fit at the page bottom, 
BOTH move to the next page together (never Totals alone, never footnote 
isolated).

NOTE on whitespace (Issue 4): The page break between the two matrices means 
some whitespace before the Commitment matrix is expected (client approved the 
page break). But Totals+footnote should not create a near-empty page on their 
own — binding them together minimizes that. Keep the existing minPresenceAhead 
on headers so the matrix header doesn't orphan either.

PART B — Footnote font (Cosmetic #3):
Change the footnote style from fontSize 7 to fontSize 8, and reduce marginTop 
from 10 to 4 (bring it closer to the table). Keep marginBottom for separation 
from the next section. Apply in both matrices:
    { fontSize: 8, color: "#475569", marginTop: 4, marginBottom: 10, fontStyle: "italic" }

CONSTRAINTS:
- Group ONLY the Totals row + footnote in one <View wrap={false}>.
- Keep data rows above ungrouped (they flow/fill the page).
- Footnote fontSize 8, marginTop 4.
- Apply to BOTH matrices identically.
- Do NOT change the page break between matrices (keep it).
- Do NOT touch pageSetup.ts, page size, margins, or backend.
- Only edit this one file. Show the grouped Totals+footnote and the footnote 
  style for both matrices.
