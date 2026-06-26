File to modify: frontend/src/app/review-history/page.tsx

Two changes per client confirmation:

CHANGE 1 — Re-label the sample dropdown:
The dropdown label currently reads "Sample / Review Name". Change the label 
text to "Sample Name". (Only the visible label/heading text for this dropdown — 
do not change the dropdown's data, options, or behavior.)

CHANGE 2 — Verify the dropdown only lists samples that have finalized reviews:
The dropdown options are built from the fetched review history rows' SampleName 
values. Since the Review History data is already filtered to finalized reviews 
(Review_finalized_date IS NOT NULL), the dropdown should already only contain 
samples that have finalized reviews. 
- Confirm this is the case by reading the code: are the dropdown options derived 
  ONLY from the finalized rows returned by getReviewHistory(), or from a separate 
  unfiltered samples source?
- If derived from the finalized rows: report "already correct, no change needed".
- If derived from a separate/unfiltered source: STOP and tell me before changing 
  anything — do not modify the data source without confirming with me first.

Modify ONLY this file. For Change 1, only the label text. Do NOT touch backend, 
data fetching, columns, pagination, or anything else. After editing, tell me: 
(a) the new label text, (b) whether the dropdown is already finalized-only or 
needs a change.
