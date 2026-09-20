Before scoping "Unsatisfactory Transactional Ratings" from scratch, check whether the underlying data already exists somewhere that gives us a design hint. READ-ONLY, no edits.

1. The Reports page has an existing "CRM Findings and Observations" or similar report with a column/section for "UNSATISFACTORY TRANSACTIONS" — does that give any structural hint (columns, grouping) for what "Unsatisfactory Transactional Ratings" should look like as its OWN report?
2. Find where "unsatisfactory" transaction/rating data is captured in the Review Form (CrmRatingsSection.tsx or similar — the same section referenced before as the data-entry source). What fields exist there (transaction type, rating, comments, date)?
3. Check the backend table(s) that store this data (grep for "Unsatisfactory" or "Transactional" in SqlReviewRepository.cs). What columns exist?
4. Check discovery/ folder or legacy Access artifacts for any report definition, query, or form reference related to "Unsatisfactory Transactional Ratings" — similar to how we found the Checklist subform reference.
5. Is there a natural column structure this data suggests (e.g. similar to CRM Findings for Management's Component/Code/Comments pattern, or something transaction-specific like Transaction Type/Date/Amount/Rating/Comments)?

Report what data exists and whether it suggests an obvious report design, or whether this genuinely needs a Geoff-provided prototype/spec before building. Do NOT propose or write a fix yet.
