Multi-file edit: fix Scorecard ID wrapping in all three reports.
Files: InitialMemoPDF.tsx, FinalMemoPDF.tsx, ReviewPDF.tsx

The Scorecard ID needs to wrap naturally within its cell. 

=== MEMOS (InitialMemoPDF.tsx + FinalMemoPDF.tsx) — remove the hyphenWrap hack ===
Both memos inject "-\u200b" after every hyphen via hyphenWrap() to force 
wrapping. This creates visible artifacts. Remove the injection and rely on 
natural wrapping (the cells already have wordBreak).

In BOTH memos, change hyphenWrap so it does NOT inject the zero-width space — 
just return the cleaned value:
    function hyphenWrap(v?: any): string {
      const s = out(v);
      return s
        .replace(/\u00a0/g, " ")
        .replace(/[\u2010\u2011\u2012\u2013\u2014\u2212]/g, "-");
      // removed: .replace(/-/g, "-\u200b")
    }
The Scorecard Assessment cell already has wordBreak "break-word" and the 
Account Info cell has wrapAnywhere (breakAll), so IDs will still wrap naturally 
at hyphens/characters within the cell width — without the injected artifacts.

=== CAS LINESHEET (ReviewPDF.tsx) — enable wrapping in Account Info ID cell ===
The Account Information Scorecard ID cell has wrap={false}, which prevents 
wrapping. Remove wrap={false} and add wordBreak so it wraps within the 25% 
cell:
Change:
    <Text wrap={false} style={[styles.tableCellValueTiny, {flexBasis: "25%", fontSize: 10, lineHeight: 1.1, minWidth: 0}]}>
      {scorecardId}
    </Text>
to:
    <Text style={[styles.tableCellValueTiny, {flexBasis: "25%", fontSize: 10, lineHeight: 1.1, minWidth: 0, wordBreak: "break-all"}]}>
      {scorecardId}
    </Text>
(Remove wrap={false}, add wordBreak: "break-all" so the ID wraps within the cell.)

For the Scorecard Assessment ID cell in ReviewPDF (flexBasis 30%, no wrap 
prop): add wordBreak: "break-all" to ensure it wraps too:
    <Text style={[styles.tableCellValueSmall, {flexBasis: "30%", fontSize: 10, lineHeight: 1.2, wordBreak: "break-all"}]}>
      {r.id || ""}
    </Text>

CONSTRAINTS:
- Memos: only remove the "-\u200b" injection line in hyphenWrap (keep the 
  other replacements). Keep the cells' existing styles.
- Linesheet: remove wrap={false} + add wordBreak on the two Scorecard ID cells.
- Do NOT change widths, other columns, or data.
- Apply the memo change to BOTH InitialMemoPDF and FinalMemoPDF identically.
- Only edit these three files. Show the hyphenWrap change (both memos) and the 
  two Linesheet cell changes.
