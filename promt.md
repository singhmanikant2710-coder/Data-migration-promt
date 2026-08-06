READ-ONLY. Diagnostics only. Do not change anything.

File: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

BUG 4 overlap: MatrixCommitment CAS PD cells (4.25% width) still overflow. 
Font 8 helped but large values still don't fit. Need to understand value range 
and fix properly.

Show:
1. The fmt() function (current — minFraction 1, maxFraction 2). For a large 
   value like 189 ($MM) it outputs "$189.1"; for 1234.56 it outputs 
   "$1,234.56" (very wide). Confirm the widest possible output.
2. The CAS PD data cell + Totals cell current style — flexBasis 4.25%, 
   fontSize 8, textAlign center. Does styles.td have overflow handling? Show 
   styles.td (overflow, wordBreak, whiteSpace if any).
3. Does styles.table have overflow: "hidden"? (Confirmed earlier yes.) Does 
   that clip cell overflow, or do cells overflow within the row?
4. The printable width per CAS PD cell: 4.25% of 720pt = ~30.6pt. At fontSize 
   8, how many characters fit (~2pt/char = ~15 chars)? So "$1,234.56" (9 
   chars) should fit at font 8 — is the overflow from the Totals row values 
   being LARGER (grand totals, e.g. "$5,678.9")? Show a few actual colTotal 
   values' magnitude if visible, or confirm the Totals row has the biggest 
   numbers (grand column totals) which are wider than individual cells.
5. Is there any wordBreak: "break-word" causing values to WRAP to two lines 
   (breaking mid-number) instead of staying one line? Show styles.td 
   wordBreak/overflow.

Do not edit anything. Show fmt() widest output, cell overflow/wordBreak 
styles, and whether Totals-row values are the largest (widest). Findings only.
