READ-ONLY — do not propose or apply any fix.

Your own report contradicts itself on F6/F7 (MonthSummaryTable.tsx /
monthSummaryRegistry.ts computed quotient for MinTangibleNetWorth): the
location table (section 2) says it "overrides canonical value," but the
key×location table says "gap-fill only." These can't both be true.

Quote the EXACT code at MonthSummaryTable.tsx:666-682 and the matching
render/save logic verbatim. Specifically answer:

1. When a user EDITS this cell in the UI and hits save, what value is
   actually included in the outgoing PUT payload — the literal value the
   user typed, or a value recomputed via computeMinTnwForRowLocal
   (TotAdjustedLiab ÷ MaxAdjustedDebtTNWLimit)? Trace the onChange/onBlur/
   onSave handler for this specific editable column, not just the render
   path.

2. Confirmed observation to explain: Nationwide Specialty Finance Inc,
   Min Tangible Net Worth, monthKey 202601. Before my edit, DB held
   strCovenantActual = 26207.215706666666. I edited the UI field to
   remove the decimal portion and saved. After refresh, the UI showed
   26207.215706666666 again — unchanged from before my edit — and the DB
   query confirms strCovenantActual is still 26207.215706666666 (not
   whatever integer I typed). Does your traced code explain this as (a)
   the payload never contained my typed value in the first place because
   the input is bound to the computed quotient, or (b) the payload did
   contain my typed value, but something recomputed and overwrote it
   server-side after write?

Report only, with exact quotes. Do not propose a fix.
