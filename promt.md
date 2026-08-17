Single-file edit: frontend/src/app/load-samples/page.tsx

Fix edit-mode horizontal overflow (right columns Type/Target + Search cut off) 
and blocked vertical scroll on the Load Samples page, plus add a missing 
"Select Type" placeholder. Tightly scoped — only these.

=== FIX 1: Grid box overflow (stop clipping cut-off content) ===
The parent grid outer box uses overflow-hidden, which clips the right-side 
columns in edit-mode instead of letting the inner horizontal scroll reach 
them. Change the parent grid outer box so it doesn't clip:
Find:
    <div className="mt-3 border border-slate-200 rounded-lg overflow-hidden shadow-sm">
Change overflow-hidden to allow the inner scroll container to work (remove the 
clipping):
    <div className="mt-3 border border-slate-200 rounded-lg shadow-sm">
(Removing overflow-hidden lets the inner overflow-x-auto scrollbar be reachable 
and stops the right columns being clipped. The rounded corners are cosmetic; if 
needed keep rounded-lg but drop overflow-hidden.)

=== FIX 2: Reduce wasted width on the EIC edit control ===
The EIC edit cell wraps a w-32 (128px) control inside a w-64 (256px) div — the 
256px wrapper wastes ~128px and inflates the row width. Make the wrapper match 
the control (or remove the fixed wrapper). Change the EIC wrapper:
Find the EIC edit wrapper div: className="w-64" wrapping the SearchableSelect 
(which is className="w-32").
Change the wrapper from w-64 to w-40 (160px) so it fits the control without 
wasting space:
    <div className="w-40"> ... <SearchableSelect className="w-32" ... /> ... </div>
(This trims ~96px from the row width without changing the control itself.)

=== FIX 3: Add "Select Type" placeholder to the Type dropdown ===
The Type <Select> first option is empty (<option value=""></option>), so the 
box shows only an arrow with no placeholder — inconsistent with EIC ("Select 
EIC Name") and Target ("Select Target"). Add placeholder text:
Find:
    <option value=""></option>
(within the Type Select) and change to:
    <option value="">Select Type</option>
(Do NOT change the other Type options — "Examination", "Continuous" (value 
"Continous"), "CCL", "Other" (value "Others") — those keep the #176 label fix.)

CONSTRAINTS:
- Only these three changes in this one file.
- FIX 1: remove overflow-hidden from the parent grid outer box only.
- FIX 2: EIC edit wrapper w-64 -> w-40 only.
- FIX 3: add "Select Type" text to the empty Type option only.
- Do NOT change the Type option labels/values (keep #176 fix), other columns, 
  widths of Target/dates, DataTable, or globals.css.
- Only edit this one file. Show the three changes.
