Attached: bcat_formats.txt (legacy Format property of every covenant and
custom-field control on all frm0XX forms and rpt0XX reports) and the
ACA 202603 SQL output.

STEP 1 — READ-ONLY analysis (report it first, in the same response):
a. For each industry form, list the Format property of txtCustomField1..10
   and covenant controls. Is custom-field format fixed PER SLOT per
   industry (not per label)? Show the table.
b. Which formats have a zero section that renders 0 as blank.
c. For frm005/frm004 Cash & Charge-offs controls: control source, Format,
   Locked/Enabled.

STEP 2 — FIX (generic, based only on Step 1 evidence):
1. Custom fields: replace our label-based rule ("%" in label -> %, else $)
   with the legacy per-industry, per-slot Format from Step 1. Where
   legacy has no Format, show the value as stored (no $, no %).
2. Zero/blank: apply each control's legacy Format including its zero
   section (legacy blank for 0 -> we show blank). NULL -> blank.
3. Indirect + Direct Auto Cash & Charge-offs (mapAutoFace only):
   - one row per item, $ and % side by side, legacy order: Cash
     Collections $ | Net C/O $ | 60+ DPD $ + % | YTD Net C/O $ |
     Net C/O TTM $ + % | Discount/Reserve $ + % | Reserve Coverage
   - only 60+ DPD $ editable
   - % fields x100 like Access Percent (per60DPD 0.0714 -> 7.14%)
   - Reserve Coverage as legacy shows it (0.00%)
Do not change values, calculations, persistence, or anything not listed.
Build, tests, do not commit.

REPORT FORMAT (mandatory):
- STEP 1 tables
- ADDED / REMOVED (file:line)
- BEHAVIOUR CHANGE incl. NULL and zero case
- NOT TOUCHED
- List of customers from test-plan.md whose custom-field display changes
