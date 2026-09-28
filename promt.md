FINAL GENERIC COVENANT FIX — replaces order/cap-based selection.

Evidence: legacy forms bind txtCovenantName{i} -> tblMain.strCovenantName{i}
and txtCovenantActual{i} -> dblCovenantActual{i}(Formatted), slots 1..4
(DirectAuto/IndirectAuto 1..5). Data: many customers have 7-10
covenants (cap of 6 drops valid ones, e.g. WESTLAKE "Other 1 (%)"), and
13 customers have 2-3 covenants on the same intCovenantOrder.

RULE (all industries, all surfaces: Top Strip, Monthly Summary, Rolling
24, Fiscal YTD, Detail grid, PDF, CSV, payload AND fallback paths):
1. Columns = non-blank tblMain.strCovenantName1..N of the selected
   month, in slot order. N = 5 for DirectAuto/IndirectAuto, else 4.
2. Value = tblMainCovenants.strCovenantActual of the covenant with the
   SAME NAME, same customer + month. If no such row, use
   dblCovenantActual{i}. NULL -> "—".
3. Format = that covenant's strCovenantFormat (existing rules).
4. Remove the covenant cap and all intCovenantOrder-based
   selection/filters (backend ~:87-98 / allowedCovSlots, payload
   Order-100 filters, report ord filter). Order 0, 5+ and duplicate
   orders no longer matter.

EVIDENCE & SAFETY (mandatory):
- Quote legacy control sources for each surface changed.
- Regression before/after on: WESTLAKE (7 covenants), ATHENS (order 0),
  ECLIPSE (order 5/6), MAMMOTH (normal), MIDDLE GEORGIA and TBS
  FACTORING (duplicate orders), MDR CONSTRUCTION (orders 5-8).
- STOP if any fixed/non-covenant column changes.
- No customer/label-specific code. Bump payloadVersion.
Do not change values, calculations, persistence. Build, tests, do not
commit.

REPORT FORMAT (mandatory):
- ADDED / REMOVED (file:line)
- BEHAVIOUR CHANGE per surface incl. NULL case
- Regression table: customer | covenant columns before | after
- NOT TOUCHED
