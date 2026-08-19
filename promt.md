Single-file edit: frontend/src/app/load-samples/page.tsx

Fix (#176 Item 1): The Type dropdown's first option has no label, so no "Select..." default shows. Add the label.

BEFORE:
<option value=""></option>
AFTER:
<option value="">Select...</option>

CONSTRAINTS:
- ONLY add the label text "Select..." to this one empty option.
- Do NOT change any other option, the onChange logic, or value handling.
- Do NOT touch any other file.
- Show the diff.
