Single-file edit: frontend/src/components/pdf/ReviewPDF.tsx (CAS Linesheet)

Fix the fixed-header bleed AND orphan on the Scorecard Assessment and Account 
Information tables. Root cause: the header uses "fixed", which in react-pdf 
renders it on EVERY page (page-level), causing:
- BLEED: Scorecard Assessment header appears on later non-table sections.
- ORPHAN: Account Information header renders at a page bottom with no rows 
  beneath, then repeats.

Since these tables flow inline (not in dedicated Pages), "fixed" is the wrong 
tool. Remove "fixed" from the header rows, and instead keep the header 
together with its first data row so the header never appears alone.

For BOTH the Scorecard Assessment table AND the Account Information table:

STEP 1 — Remove "fixed" from the header row View:
    <View style={styles.tableRow} wrap={false} fixed>   ...header...   </View>
becomes:
    <View style={styles.tableRow} wrap={false}>   ...header...   </View>
(Remove ONLY the fixed prop. Keep wrap={false}.)

STEP 2 — Prevent the header from being orphaned at a page bottom: wrap the 
header row together with the first data row in a single wrap={false} group, so 
if they don't fit at the page bottom, BOTH move to the next page together (the 
header never sits alone).

Restructure each table body from:
    <View style={styles.table}>
      <View style={styles.tableRow} wrap={false}>...header...</View>
      {rows.map((r,i) => <View style={styles.tableRow} wrap={false} key=...>...row...</View>)}
    </View>
to:
    <View style={styles.table}>
      {rows.length > 0 ? (
        <>
          {/* header + first row kept together so header can't orphan */}
          <View wrap={false}>
            <View style={styles.tableRow}>...header...</View>
            <View style={styles.tableRow}>...first row (rows[0])...</View>
          </View>
          {/* remaining rows flow normally, each wrap={false} */}
          {rows.slice(1).map((r,i) => (
            <View style={styles.tableRow} wrap={false} key={...}>...row...</View>
          ))}
        </>
      ) : (
        <>
          <View style={styles.tableRow} wrap={false}>...header...</View>
          <View style={styles.tableRow} wrap={false}>...empty placeholder row...</View>
        </>
      )}
    </View>

The key change: the header is grouped with the first row inside a wrap={false} 
block (so they stay together — no orphan), and "fixed" is removed (so no bleed 
onto other sections). The trade-off is the header no longer repeats on every 
page of a multi-page table — but that also stops the bleed/orphan, matching a 
clean single-header table.

CONSTRAINTS:
- Apply to BOTH the Scorecard Assessment table and the Account Information 
  table in ReviewPDF.tsx.
- Remove "fixed"; keep wrap={false} on rows.
- Group header + first data row in a shared wrap={false} block.
- Keep column widths, cell content, Scorecard ID wrapAnywhere, and formatting 
  unchanged.
- Only edit this one file. Show the restructured Scorecard Assessment and 
  Account Information tables.
