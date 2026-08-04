Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Four small, specific cosmetic changes. Each is bounded — do NOT loop or 
search broadly. Make exactly these four changes and stop.

=== CHANGE 1: Count matrix — show "0" for empty cells ===
In MatrixCount (the "PD Grade Migration by Number of Accounts" table), the 
data cells currently render an empty/NBSP value when the count is 0. Geoff 
wants empty cells to display "0" instead.
Find the cell render in MatrixCount where it outputs the count value (it 
likely does something like `{v === 0 ? NBSP : String(v)}` or 
`{v || NBSP}`). Change it so a 0 value displays "0":
  {String(v)}   // always show the number, including 0
(Apply ONLY to MatrixCount data cells. Do NOT change the totals row logic 
if it already shows numbers.)

=== CHANGE 2: Commitment matrix — hide "$0" values ===
In MatrixCommitment (the "PD Grade Migration by Commitment" table), the data 
cells currently show "$0" for zero values. Geoff wants $0 hidden (blank) in 
the green and pink sections, matching how the count table hides empties.
Find the cell render in MatrixCommitment where it outputs the money value. 
Change it so a 0 value renders blank (NBSP) instead of "$0":
  {(!v || v === 0) ? NBSP : money(v)}
(Apply ONLY to MatrixCommitment data cells. Leave the Totals row showing 
its values as-is unless it also shows $0 — keep totals showing real amounts.)

=== CHANGE 3: Capitalize "# of Accounts" header ===
In Subreport03_DistByCount (Final PD Distribution Count), the column header 
currently reads "# of Accounts". Change it to all caps:
  "# OF ACCOUNTS"

=== CHANGE 4: Detail table — Direction wider, Commitment narrower ===
In the DETAIL table (page with CUSTOMER NAME (REVIEW ID) | BANK PD | CAS PD | 
DIRECTION | COMMITMENT), Geoff wants the Direction column wider and the 
Commitment column narrower.
Show the current width styles for the DIRECTION and COMMITMENT columns 
(likely wDirection and wCommit or similar), then increase DIRECTION's width 
and decrease COMMITMENT's width by a matching amount so the row still sums 
to 100%. For example, if Direction is 10% and Commitment is 25%, change to 
Direction 18% and Commitment 17% (adjust based on the actual current values 
— keep the total unchanged).

CONSTRAINTS:
- Make ONLY these four changes. Do NOT touch other tables, colors, data 
  logic, or the % columns (those are a separate batch).
- For Change 4, only adjust the DIRECTION and COMMITMENT column widths in 
  the detail table; keep all detail columns summing to 100%.
- Do NOT loop or repeat. Show each changed line/style once and stop.
- Only edit this one file. List the four changes made.
