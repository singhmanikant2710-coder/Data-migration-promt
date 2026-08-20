Single-file edit: frontend/src/app/load-samples/page.tsx

Fix (#176a, per Geoff): All three dropdowns (EIC, Type, Target) should show "Select..." as a default prompt but NOT allow "Select..." to be chosen as an option. EIC and Target use SearchableSelect where "Select..." is a placeholder (not selectable) — correct. Type uses a native <Select>, where the "Select..." <option> is currently selectable (wrong). Make it a non-selectable placeholder by adding disabled + hidden to that option, so it shows as the default prompt but can't be picked, matching the other two dropdowns.

BEFORE:
<option value="">Select...</option>

AFTER:
<option value="" disabled hidden>Select...</option>

CONSTRAINTS:
- ONLY change this one <option> (add disabled hidden).
- Do NOT change the other Type options (Examination, Continous, CCL, Others).
- Do NOT change the Select's value/onChange logic.
- Do NOT touch EIC, Target, or any other file.
- Show the diff.
