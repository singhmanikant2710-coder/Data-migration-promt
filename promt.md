Single-file edit: frontend/src/app/load-samples/page.tsx

Fix two display typos in the Sample "Type" dropdown WITHOUT changing the saved 
values (the value = label currently, and the values "Continous"/"Others" are 
persisted to the DB and used in filters/comparisons elsewhere, so we keep the 
VALUE unchanged and only fix the visible LABEL).

In the Sample Type <Select>, separate the display label from the value for the 
two options with typos — keep value as-is, fix only the label text shown:

Change:
    <option value="Continous">Continous</option>
to:
    <option value="Continous">Continuous</option>

Change:
    <option value="Others">Others</option>
to:
    <option value="Others">Other</option>

(The value attribute stays "Continous" and "Others" — so the stored value, 
the equality checks like r.sample_type === "Continous", the API filter 
searchSamples({ type: "Continous" }), and existing saved data are ALL 
unaffected. Only the text the user sees in the dropdown is corrected.)

CONSTRAINTS:
- ONLY change the display text (between <option>...</option>), NOT the value 
  attribute.
- Do NOT change any equality checks, filters, the isContinuous regex, saved 
  payload, or backend.
- Do NOT touch other options (Examination, CCL) — they have no typos.
- Only edit this one file. Show the two updated <option> lines.
