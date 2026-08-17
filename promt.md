Multi-file edit: InitialMemoPDF.tsx + FinalMemoPDF.tsx

Apply the SAME header bleed/orphan fix we just did in CAS Linesheet to the 
Account Information table in both memos. The memos currently use "fixed" on the 
header (which bleeds/orphans, page-level in react-pdf) AND batch rows with 
break={ci > 0} — both must change to match the working CAS Linesheet pattern.

Restructure the Account Information table in BOTH memos:

STEP 1 — Remove batching. Do NOT chunk rows into batches with break={ci > 0}. 
Render ONE single table container:
    <View style={styles.table}>
      ...header + rows...
    </View>
(Remove batches.map, chunk(rows, 12/15), and per-batch break={ci > 0}.)

STEP 2 — Remove "fixed" from the header row (it causes page-level bleed/orphan):
    <View style={[styles.tr, styles.trHeader]} wrap={false} fixed>
becomes:
    <View style={[styles.tr, styles.trHeader]} wrap={false}>
(Remove ONLY fixed. Keep wrap={false}.)

STEP 3 — Group the header row with the FIRST data row in a single wrap={false} 
block so the header can never orphan at a page bottom (same as CAS Linesheet):
    <View style={styles.table}>
      {rows.length > 0 ? (
        <>
          <View wrap={false}>
            <View style={[styles.tr, styles.trHeader]}>...8 header cells...</View>
            <View style={[styles.tr, rows.length === 1 ? styles.trLast : {}]}>
              ...8 data cells for rows[0] (accountNumber, scorecardIdBank via 
              hyphenWrap+wrapAnywhere, bankPd, bankLgd, casPd, casLgd, balance, 
              commitment)...
            </View>
          </View>
          {rows.slice(1).map((a, i) => (
            <View key={`acc-${i+1}`} wrap={false} style={[styles.tr, i === rows.slice(1).length - 1 ? styles.trLast : {}]}>
              ...8 data cells for a...
            </View>
          ))}
        </>
      ) : (
        <>
          <View style={[styles.tr, styles.trHeader]} wrap={false}>...header...</View>
          <View style={[styles.tr, styles.trLast]} wrap={false}>...empty placeholder row...</View>
        </>
      )}
    </View>

This matches CAS Linesheet exactly: single table (no batching), header NOT 
fixed, header grouped with first row (no orphan), remaining rows flow with 
wrap={false}. No bleed (fixed removed), no orphan (header+first row grouped), 
no mid-page repeat (no manual re-header, no fixed).

CONSTRAINTS:
- Apply to the Account Information table in BOTH InitialMemoPDF.tsx and 
  FinalMemoPDF.tsx.
- Remove batching (chunk + batches.map + break), remove "fixed", group 
  header+first row.
- Keep column widths (cols8Accounts), header cell text, Scorecard ID 
  hyphenWrap+wrapAnywhere, currency formatting unchanged.
- Do NOT touch other tables (Scorecard Assessment, Findings) unless they have 
  the same fixed-header issue — if the Scorecard Assessment table in the memos 
  also uses "fixed", apply the same header+first-row grouping there too (remove 
  fixed, group header with first row).
- Only edit these two files. Show the restructured Account Info table in both 
  memos.
