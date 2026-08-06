Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Make the charts block atomic so it never splits across pages (fixes charts 
splitting on large datasets like 354). This is a generic fix — works for all 
datasets.

In the DistCharts component, its root View currently has no wrap prop (so it's 
splittable). Add wrap={false} to that root View:

Current:
    return (
      <View style={{ display: "flex", flexDirection: "row", gap: 8 }}>
        <PDBarChart ... />
        <PDBarChart ... />
      </View>
    );

Change to:
    return (
      <View wrap={false} style={{ display: "flex", flexDirection: "row", gap: 8 }}>
        <PDBarChart ... />
        <PDBarChart ... />
      </View>
    );

(DistCharts is ~180pt tall — always less than one page height of 484pt. So 
wrap={false} safely moves the entire chart block to the next page if it 
doesn't fit at the bottom of the current page, instead of splitting the charts 
across two pages. Never splits.)

CONSTRAINTS:
- ONLY add wrap={false} to the DistCharts root View.
- Do NOT change the charts' content, the PDBarChart components, or chartHeight.
- Do NOT change the <View break /> before MatrixCommitment (keep it).
- Do NOT change the matrices, footnote, widths, or pageSetup.
- Only edit this one file. Show the DistCharts root View with wrap={false}.
