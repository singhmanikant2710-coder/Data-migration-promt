Single-file edit: frontend/src/components/pdf/ScorecardResultsPDF.tsx

Change the header title font size from 20 to 14 (to match CRM Summary).

BEFORE (headerTitle style): fontSize: 20
AFTER  (headerTitle style): fontSize: 14

CONSTRAINTS:
- ONLY change fontSize 20 -> 14 in the headerTitle style object.
- Do NOT change color, fontWeight, or anything else.
- Do NOT touch any other file.
- Only edit this one file. Show the diff.

- Single-file edit: frontend/src/components/pdf/ScorecardResultsPDF.tsx

The right-hand header currently shows the caption (sample name). Change it to show the report run date, matching CRM Summary. This component does NOT receive a generatedOn prop, so use the current date via formatDate (which accepts an ISO string). Confirm formatDate is imported from ./pageSetup (it is used by sibling PDFs) — if not imported, add it to the existing pageSetup import.

BEFORE:
<Text style={styles.headerMeta}>{out(caption)}</Text>

AFTER:
<Text style={styles.headerMeta}>{out(formatDate(new Date().toISOString()))}</Text>

CONSTRAINTS:
- ONLY change this one header line.
- Do NOT remove the `caption` variable (it may be used elsewhere) — just stop using it in this header line.
- Do NOT change headerMeta style or any other block.
- Do NOT touch any other file.
- Only edit this one file. Show the diff.

Single-file edit: frontend/src/components/pdf/ScorecardResultsPDF.tsx

Replace the footer (which has the First Horizon logo + "CAS RiskReview" brand) with the centered text style used by CRM Summary and PD Migration reports: a centered "[Report Name] • Page X of Y" line. Remove the logo Image and the brand text.

BEFORE:
<View style={styles.footer} fixed>
  <View style={styles.footerInner}>
    <View style={styles.footerLeft}>
      <Image src="/assets/FHB_Logo.png" style={styles.footerLogo} />
      <Text style={styles.footerBrand}>CAS RiskReview</Text>
    </View>
    <Text style={styles.footerText} render={({ pageNumber, totalPages }) => `Page ${pageNumber} of ${totalPages}`} />
  </View>
</View>

AFTER:
<View style={styles.footer} fixed>
  <View style={{ borderTopWidth: 1, borderTopColor: colors.divider, borderTopStyle: "solid", paddingTop: 4 }}>
    <Text
      style={{ fontSize: 8, color: "#475569", textAlign: "center" }}
      render={({ pageNumber, totalPages }) => `CRM Scorecard Results • Page ${pageNumber} of ${totalPages}`}
    />
  </View>
</View>

CONSTRAINTS:
- Replace ONLY the footer's inner content as shown. Keep the outer <View style={styles.footer} fixed> wrapper.
- Remove the FH logo Image and the "CAS RiskReview" brand text.
- If `colors` is already imported from ./pageSetup (it is used elsewhere), use colors.divider as shown; otherwise use "#e2e8f0".
- Leave the now-unused footer styles (footerInner, footerLeft, footerLogo, footerBrand, footerText) in place for now — do NOT delete them (removing could risk other references; we can clean up later).
- Do NOT touch any other file.
- Only edit this one file. Show the diff.
