READ-ONLY. Diagnostics only. Do not change anything.

I need to add a "% OF COUNT" column to the "PD Migration Totals by Account" 
table (Subreport01_Count, ~line 381) and a "% OF COMMITMENT" column to the 
"PD Migration Totals by Commitment" table (Subreport02_Commitment, ~line 403). 
The percentage = each row's value ÷ the grand total of that column, shown 
as a percentage.

Report back (no edits):

1. For Subreport01_Count: show the exact data array it maps over (the prop 
   name and its shape, e.g. pairs with fromPd/toPd/count). Confirm whether 
   a grand total of all `count` values is already available anywhere (a 
   totals variable, a passed-in prop), OR whether I need to compute it by 
   summing the array in the component.

2. For Subreport02_Commitment: same — show the data array (fromPd/toPd/
   sumCommitment), and whether a grand total of all commitment values is 
   already available or must be computed.

3. Show the current header row and body row JSX for BOTH tables (the exact 
   column <Text> elements and their width styles: w35/w35/w30), so I can 
   see where to insert the new percentage column and how to rebalance widths.

4. Confirm the money()/number formatting helpers available in the file 
   (e.g. money() for commitment) and whether there's an existing percent 
   formatting helper, or if I should format inline (e.g. toFixed(1) + "%").

Do not edit anything. Findings only.
