READ-ONLY. No code changes. Build a LEGACY SPEC of the BCAT Access app,
industry by industry, from these sources:
- Discovery/legacy_ui/bcat_ui_spec.csv — every control on every frm0XX
  form and rpt0XX report: name, type, section, Left/Top/Width,
  ControlSource, Caption, Enabled, Locked, Visible, Format,
  DecimalPlaces, BackColor, TabIndex, Parent (label -> control).
- Discovery/extraction_output (VBA incl. funSave / cmdAddNewMonth,
  saved queries, schemas).
- tblMain calculated-field expressions (pasted at the end — ground truth).

For EACH of the 13 industries (frm001..frm013 Main/CurrentEdit/
CurrentYear/Rolling24 + rpt0XX):
1. Screen layout in visual order (sort by Top, then Left): Summary
   strip, Month/TTM block, Cash & Charge-offs block, Equity/Liabilities/
   Availability block, monthly grid, report columns. Use the attached
   label (Parent/Caption) as the field label.
2. Per field: label | source column | editable? (Enabled=True and
   Locked=False; also note BackColor of editable cells) | format |
   type = INPUT / LIVE (tblMain calculated field, same-row) / ON-SAVE
   (funSave/queries: YTD, TTM, prior month, averages) / COVENANT /
   CUSTOM / DISPLAY-ONLY (bound to a column another control edits).
3. Add New Month: exactly what legacy writes. Save: funSave steps in
   order (which columns, which months).
4. Differences between industries that look identical (e.g. 004 vs 005).

Write one file per industry: docs/legacy-spec/<NNN>-<industry>.md, plus
docs/legacy-spec/README.md with the shared rules. In chat, print only a
short summary per industry (field counts, editable counts, anything
unclear).

If you need data from the databases to settle anything, DO NOT guess:
list it at the end under "QUERIES NEEDED" (Access SQL and/or SQL Server
SQL, ready to run). I will run them and paste the output back.

Do not change code. Do not commit.

LEGACY tblMain CALCULATED FIELDS (ground truth):
<<< paste the formula list here >>>
