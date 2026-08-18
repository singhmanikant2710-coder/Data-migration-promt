Single-file edit: frontend/src/app/load-samples/page.tsx

Bug #176 follow-up: Geoff wants "Select Type" removed from the Type dropdown. 
Revert the placeholder text back to an empty option.

Change:
    <option value="">Select Type</option>
to:
    <option value=""></option>

CONSTRAINTS:
- ONLY change the "Select Type" option back to empty text.
- Do NOT change the other Type options (keep #176 fixes: Continuous, Other).
- Do NOT change values or anything else.
- Only edit this one file. Show the changed option.
