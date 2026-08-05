READ-ONLY. Diagnostics only. Do not change anything.

The client wants to restructure the matrix headers and add footers. I need 
to understand the current header structure and the reference layout before 
editing. Investigate (no edits):

1. In MatrixCount and MatrixCommitment: show the FULL current header row 
   structure — the "BANK PD" label cell, the 1-16 column cells (now showing 
   "13/SM" etc.), and the "Totals" cell. Show the exact JSX, the cell 
   styles/widths, and the height-related styles (padding, fontSize, 
   lineHeight) on styles.tr, styles.trHeader, styles.thDark.

2. The "BANK PD" header cell alignment: how is it currently aligned (left/
   center)? And how are the data row PD-number cells (the first column, 
   showing fromPd) aligned? The client wants "BANK PD" aligned with those PD 
   rating numbers below it.

3. For adding a "CAS PD" grouping header ABOVE the 1-16 columns: is the 
   header currently a single row? To add a "CAS PD" label spanning columns 
   1-16, I'd need a two-row header (top row: "CAS PD" spanning the rating 
   columns; bottom row: 1-16). Show whether the current header is one row 
   and what the column width structure is (so I can build a spanning header).

4. Header row height/thinness: what makes the header tall right now — is it 
   the "13/SM", "14/SUB" labels wrapping to two lines (making the row taller)? 
   Show the fontSize and any wrapping. The client wants it thinner so there's 
   room for a footer; if we can shrink it (smaller font, single line), we 
   keep the SM/SUB/DFUL/LOSS labels.

5. For the matrix FOOTER (a note below each matrix): is there an existing 
   note/footer pattern? Show if the report (or the reference) has a note like 
   "CAS PD Rating totals represent the committed exposure... Red cells 
   indicate PD downgrades... green cells indicate PD upgrades..." — I need to 
   know the exact footer text the client wants (from the reference PD 
   Migration report). If it's not in code, I'll ask the client for the exact 
   text.

Do not edit anything. Show the header structure, alignment, height sources, 
and any existing matrix-note pattern. Findings only.
