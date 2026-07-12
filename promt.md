Approved — apply the plan, with these explicit guarantees:

1. CRITICAL — do NOT change the save payload. The `info` field must continue to be sent in the crmFindingsAndRatings save payload exactly as it is today (`info: f.info ?? null`), preserving any existing saved values in the DB. We are only removing the INPUT from the UI; we are not deleting or nulling the stored data.

2. Confirm explicitly in your summary: after this change, saving a review does NOT wipe or overwrite any existing row.info values in the database.

3. The category cell is display-only and read-only in BOTH edit mode and read-only mode (no input in either).

Apply the single-file change to CrmFindingsAndRatingsSection.tsx (categoryMap + INFO cell + colgroup widths). Show me the diff. STOP if another file needs changing.
