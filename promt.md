Single-file edit: frontend/src/app/load-samples/page.tsx

Make all create-mode input fields consistent width (w-40) so they look equal. Currently EIC Name and Target (BU) use w-48 while Type uses w-40. Bring EIC and Target down to w-40 to match Type (and the date inputs at w-[10rem] ≈ w-40).

EIC Name column SearchableSelect (wrapper + component):
BEFORE: className={isCreating ? "w-48 min-w-0" : "w-28 min-w-0"}
AFTER:  className={isCreating ? "w-40 min-w-0" : "w-28 min-w-0"}
BEFORE: className={isCreating ? "w-48" : "w-32"}
AFTER:  className={isCreating ? "w-40" : "w-32"}

Target (BU) column SearchableSelect (wrapper + component):
BEFORE: className={isCreating ? "w-48 min-w-0" : "w-28 min-w-0"}
AFTER:  className={isCreating ? "w-40 min-w-0" : "w-28 min-w-0"}
BEFORE: className={isCreating ? "w-48" : "w-32"}
AFTER:  className={isCreating ? "w-40" : "w-32"}

Type dropdown: already w-40 in create mode — leave as is (this is the target width all fields should match).

CONSTRAINTS:
- ONLY change EIC and Target w-48 -> w-40 (4 occurrences). Type stays w-40.
- Non-creating widths stay w-28/w-32.
- Do NOT change dates, grid wrappers, DataTable, or any other file.
- Show the diff.
