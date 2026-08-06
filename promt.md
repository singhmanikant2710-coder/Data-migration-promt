READ-ONLY. Diagnostics only. Do not change anything.

File: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

UAT found 8 issues after the structural change (per-row # Changes/% Change 
columns). Investigate all before fixing:

ISSUE 1 (new column header font too small): Show the header cell style for 
the new "# Changes" and "% Change" columns vs existing headers (BANK PD, 
Totals, PD columns). Is there a smaller fontSize on the new column headers? 
Show exact style on each so I can make them identical (same fontSize, 
fontWeight, alignment).

ISSUE 2 + 8 (new columns too narrow + centering): Current widths: BANK PD 8% 
| 16 PD cols 4.5% | Totals 12% | # Changes 4% | % Change 4%. The 4% columns 
are cramped. Show all column widths (header + body) and textAlign. I want to 
widen # Changes and % Change to ~7% each by reducing PD columns (e.g. 4.5% 
-> 4%) or Totals. Also show PD column header textAlign (centering check).

ISSUE 3 + 4 + 5 (Totals split / whitespace / footnote): After removing the 
Changes/%Change rows, is the bottom Totals row still wrapped in <View wrap=
false>, or standalone? Show:
- The current structure: last data rows -> Totals row -> footnote note.
- Whether Totals row is grouped/wrapped and where the footnote sits.
- I need Totals + footnote to stay with the matrix (not isolate on next page 
  with whitespace).

ISSUE 6 (hide 0/$0 in colored cells): Show the DATA cell render in both 
matrices with the color logic. Currently colored cells (pink bg #fee2e2 when 
toPd>fromPd, green bg #dcfce7 when toPd<fromPd) show "0"/"$0". Client wants: 
in COLORED cells (toPd != fromPd), hide 0/$0 (show blank) so real changes 
pop. Show the exact cell value render + the bg color condition so I can blank 
zeros only in colored cells (keep values in white/diagonal cells as-is, or 
confirm whether diagonal 0s stay).

ISSUE 7 (subreport row shading): Show Subreport01_Count and 
Subreport02_Commitment ("PD Migration Totals by Account/Commitment" tables 
with FROM PD, TO PD, COUNT, % OF COUNT). Show each row's render and whether 
fromPd/toPd are available per row. I need to shade rows: toPd < fromPd = 
light green (#dcfce7, upgrade); toPd > fromPd = light red (#fee2e2, 
downgrade); toPd == fromPd = no shade.

Cosmetic #3 (footnote): Show current footnote fontSize and marginTop (client 
wanted font 8 and closer to table — verify if applied).

Do not edit anything. Show: new column header styles, all widths + textAlign, 
Totals/footnote grouping, colored-cell value render + color condition, 
subreport row structure + fromPd/toPd availability, footnote font. Findings only.
