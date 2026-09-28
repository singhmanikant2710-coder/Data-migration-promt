STRICT-SCOPE FIX — ConsumerFinance, DirectAuto, IndirectAuto only.
Legacy (frm004 / frm005 / frm008 + rpt004/005/008) is the source of truth.

SAFETY RULES (mandatory):
- Change ONLY items a-d below. Touch nothing else.
- Do NOT change calculations, stored values, DB data, APIs, persistence,
  or any other industry. Item e is READ-ONLY.
- Do NOT touch the "FCC TTM" no-suffix rule, covenant rules, custom-field
  "as stored" rule, selected-month clamp, or any earlier fix.
- No customer-specific code or values.
- If any change would affect another industry or an unlisted field,
  STOP and report instead of applying.

PRE-VERIFIED — do NOT fix (data issues, SQL != Access):
MARINER PBT, YTD PBT, Interest Coverage TTM, 60+ DPD %, Net C/O selector,
all covenants; SHABANA custom fields 1-4; ACA/SHABANA blank-vs-$0.

FIX (all surfaces: Summary Top Strip, Monthly Summary, Month/TTM,
Cash & Charge-offs, right rail, Fiscal YTD, Rolling 24, Detail grid,
PDF, CSV):

a. Net C/O TTM %: display the stored perNetChargeOffTTM
   (MARINER 202603 = 0.0851 -> 8.51%). UI shows 0.46% — find what it
   computes instead and replace it with the stored field. No recompute.

b. Reserve Coverage: Access Percent format, stored value x100
   (1.22374 -> 122.37%). Replace the "x" ratio format for this field.

c. "Interest Coverage TTM": must show "x" (1.88 -> 1.88x). Remove this
   label from every no-suffix set (MonthSummaryTable, DetailGrid
   /Coverage/i, BlackBookPdf RATIO_NO_SUFFIX_LABELS). Keep "FCC TTM"
   no-suffix exactly as it is.

d. ConsumerFinance (frm008) structure:
   - Month/TTM: rows and order exactly as frm008; remove the extra
     "FCC TTM" row if frm008 does not have it there.
   - Cash & Charge-offs: rows, order and visibility exactly as frm008
     (Net C/O % followed by the Principal N/R / Gross N/R selector);
     remove the standalone "Principal N/R" row if frm008 does not show
     it. Keep the Principal/Gross dropdown working exactly as today.
   Quote frm008 control sources / positions you used.

e. READ-ONLY: SHABANA 202601 curAveragePrincipalNRTTM — Access 44,413,
   SQL 4,412.50. Did our TTM mirror (AVG over window) overwrite it, and
   does it average migrated zeros that legacy stored as NULL? Report
   only, no change.

Build, tests, do not commit.

REPORT FORMAT (mandatory):
- Per item a-d: root cause (1 line) + ADDED (file:line) + REMOVED
  (file:line)
- BEHAVIOUR CHANGE per surface, incl. NULL and zero case
- NOT TOUCHED (confirm FCC TTM, covenants, custom fields, other
  industries unchanged)
- Item e findings
- Expected after fix: ACA 202603 Interest Coverage TTM 1.88x;
  SHABANA 202601 Interest Coverage TTM 1.75x, Reserve Coverage 120.91%;
  MARINER 202603 Net C/O TTM 8.51%, Reserve Coverage 122.37%
