STRICT-SCOPE FIX (generic, all customers). Touch nothing else.
Customer seen: WESTLAKE SERVICES LLC (IndirectAuto) 202604.

1. Covenant visibility: a customer's covenant is shown ONLY by its own
   intCovenantOrder (1..N). Never hide/drop a real covenant because its
   label matches a template/placeholder list (e.g. "Other 1 (%)").
   WESTLAKE: Min EBITDA/Interest (1, x, 2.57), Max CAR Ratio (2, %, 11.28),
   Other 1 (%) (3, %, 9.06) must all show on Top Strip, Monthly Summary,
   Rolling 24, Detail grid, PDF. Find and quote the line that drops it.
2. Covenant format everywhere uses strCovenantFormat: Max CAR Ratio ->
   "11.28%" in grids/PDF, "11.28" on Top Strip. If a template column
   with the same label exists, the customer covenant wins (value + format).
3. Custom fields: render the stored text EXACTLY on every path
   (payload, registry/auto order filter, Rolling 24, Detail, PDF, CSV).
   WESTLAKE: "45.1%", "142%", "$20,679,161", "8.16%". No number coercion.
   Quote the path that currently strips the symbols.
4. Reserve Coverage (ConsumerFinance, DirectAuto, IndirectAuto): follow
   legacy per surface — tile row / Top Strip / Monthly Summary / grid /
   PDF = FormatNumber(x,2)&"x" (1.4815 -> 1.48x); Cash & Charge-offs panel
   = Access Percent (148.15%). Verify against frm004/005/008 control
   sources before applying.

Do not change values, calculations, FCC TTM rule, other fixes.
Build, tests, do not commit.

REPORT FORMAT (mandatory):
- Root cause per item + ADDED / REMOVED (file:line)
- BEHAVIOUR CHANGE per surface incl. NULL case
- NOT TOUCHED

- EVIDENCE & SAFETY (mandatory):
1. Before changing anything, quote the legacy control source / format
   for EVERY surface the change touches (tile row, grid, panel, report).
   No legacy evidence = no change.
2. List every industry and surface the change affects. If it affects
   anything outside the requested scope, STOP and report.
3. Regression check: show before/after for this customer AND one
   customer from 2 other industries that use the same code path.
4. No label-, customer- or industry-specific shortcuts unless the
   legacy forms themselves differ by industry.
