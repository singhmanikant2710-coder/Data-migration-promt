Apply the Account Information table fix, but your FinalMemoPDF.tsx diff is
malformed — several rows show BOTH the old (styles.td) and new (styles.tdAcct)
<Text> cells being added together, which would create DUPLICATE cells and break
the table (especially the Balance/Commitment cells and the empty-state rows).

Re-apply carefully:
1. InitialMemoPDF.tsx: apply as proposed (add thAcct/tdAcct with padding 4, use
   them in the Account Information header + all data rows + empty state, add
   wrapAnywhere to Account # c1 and remove wrap={false} from it). Columns
   unchanged.
2. FinalMemoPDF.tsx: apply the SAME intent, but each old <Text style={[styles.th
   ...]}> / <Text style={[styles.td ...]}> in the Account Information table must
   be REPLACED (not duplicated) by the thAcct/tdAcct version. Do NOT leave both.
   Ensure exactly one <Text> per cell.
3. Confirm styles.wrapAnywhere already exists in BOTH files (it's used for
   Scorecard ID). If it doesn't exist in FinalMemoPDF, tell me before using it.
4. Do not touch any other table or shared th/td padding.

After applying, run tsc mentally and show me: the final thAcct/tdAcct style defs,
the full Account Information header + one data row + empty-state row for
FinalMemoPDF, so I can confirm there are no duplicate cells. Auto-approve OFF.
