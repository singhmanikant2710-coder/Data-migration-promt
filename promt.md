Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Color-code the cell VALUE text in both migration matrices based on their 
section background (Geoff's request):
- Values in the PINK section (background #fee2e2) → dark red text
- Values in the GREEN section (background #dcfce7) → dark green text
- Values in the diagonal / no-background cells → keep default text color

The cells already compute a background color `bg` based on toPd vs fromPd. 
In BOTH MatrixCount AND MatrixCommitment, the data cell currently sets 
backgroundColor via that `bg` logic (pink when toPd > fromPd, green when 
toPd < fromPd, undefined when equal).

For each data cell, ALSO set the text color to match:
- Add a computed text color alongside the existing bg. Where bg is currently 
  determined like:
    const bg = toPd > r.fromPd ? "#fee2e2" : toPd < r.fromPd ? "#dcfce7" : undefined;
  add:
    const fg = toPd > r.fromPd ? "#991B1B" : toPd < r.fromPd ? "#166534" : undefined;
  (#991B1B = dark red, #166534 = dark green)

- Then apply `color: fg` in the cell's style, alongside the existing 
  backgroundColor: bg. For example, in the cell style array add:
    { backgroundColor: bg, color: fg }
  (keep all other existing style properties on that cell).

Apply this to BOTH:
1. MatrixCount data cells (the "by Number of Accounts" table)
2. MatrixCommitment data cells (the "by Commitment" table)

CONSTRAINTS:
- Only add the text color (fg) to the DATA cells that already have the pink/
  green background logic. 
- Do NOT color the diagonal cells (where toPd === fromPd) — leave fg 
  undefined so they use default text color.
- Do NOT color the header row, the BANK PD label column, the Totals column, 
  or the Totals row — only the inner data cells that have the pink/green bg.
- Do NOT change the background colors themselves, the values, or any other 
  logic.
- Match the fg computation to the EXISTING bg computation in each matrix 
  (use the same toPd vs fromPd comparison that's already there).
- Do NOT loop. Make the change in both matrices' data cells and stop.
- Only edit this one file. Show the changed cell code for both matrices.
