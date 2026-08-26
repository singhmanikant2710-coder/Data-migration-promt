SINGLE-FILE, MINIMAL, BOUNDED EDIT. Only edit frontend/src/blackbook/pdf/BlackBookPdf.tsx. Show unified diff BEFORE applying. Do not run build. Do not touch other code.

CONTEXT: Legacy rolling-24 PDF shows exactly TWO sections on one page: current fiscal year (up to 12) + prior fiscal year (up to 12) = 24 months. Legacy has NO "two fiscal years back" (FY-2) section. Our app renders a third FY-2 section ("Historical Summary - {prevYear2}") that isn't in legacy and overflows to page 2. To match legacy, suppress the FY-2 section entirely.

EXACT CHANGE — gate the FY-2 section so it does not render. Find this block:

{historyYear2Only.length > 0 && (
  <View>
    <Text style={[styles.sectionTitle, { marginTop: 2, marginBottom: 1 }]}>
      Historical Summary - {prevYear2}
    </Text>
    <View style={[styles.table, { marginBottom: 0 }]} wrap={false}>
      <View>
        {renderHeader(colsHist, "history")}
        {historyYear2Only.slice(0, 12).map((r: MetricPoint, idx: number) => (
          <View key={String((r as any).monthKey || idx)}>
            {renderRow(colsHist, r, idx, "history")}
          </View>
        ))}
      </View>
    </View>
  </View>
)}

Change the condition from {historyYear2Only.length > 0 && ( to {false && historyYear2Only.length > 0 && ( so the FY-2 section is disabled (kept in code, not deleted, matching how other unused blocks are gated with `false &&`). Do not delete the block — just gate it off.

DO NOT change:
- monthlyChunks (keep [seriesYearOnly] — current-year full)
- historyYearOnly.slice(0, 12) (keep FY-1 at 12)
- Any headings, panels, styles for current/FY-1 sections

VERIFY BEFORE SHOWING DIFF:
a) Confirm only the FY-2 section's condition changed to `false && ...` and nothing else.
b) Confirm current-year is still [seriesYearOnly] and FY-1 still slice(0,12).
c) Confirm the FY-2 block is gated (not deleted) so it can be re-enabled later if the client wants it.

Show the unified diff. Apply nothing until I confirm.
