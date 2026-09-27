REGRESSION. ATHENS PAPER COMPANY INC (WholesaleTrade): the "Min Tangible
Net Worth" covenant column disappeared from Summary Top Strip and
Monthly Summary. MAMMOTH MEDICAL INC (same industry) is fine.
ATHENS covenants in tblMainCovenants: Other 1 (%) intCovenantOrder=0,
Min Tangible Net Worth =1, Min Net Income =2 (format "$").

1. READ-ONLY first: trace ATHENS through backend SummaryPayload
   (covenant slot/order assignment, cap, name matching) and frontend
   covenant filters (Order-100 slot filter, 1..N cap, registry/profile
   excludes). Quote the exact line that drops Min Tangible Net Worth and
   explain why Min Net Income survives.
2. FIX: covenant visibility = intCovenantOrder 1..N of the covenant
   itself, independent of other covenants' orders (order 0 must not
   shift slots). Generic for all customers.
3. PDF: covenant values in the PDF must follow the same rule as the
   Summary Top Strip? -> NO, keep as is until confirmed.

Build, tests, do not commit.

REPORT FORMAT (mandatory):
- ADDED / REMOVED (file:line)
- BEHAVIOUR CHANGE incl. NULL case
- NOT TOUCHED
