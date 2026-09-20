Checklist Questionnaire — Geoff's feedback fully confirmed. Implement all three pieces. Show diffs, do NOT commit.

CONTEXT: Section/Guidance columns already removed, header time already removed (both done). Now adding:

1. QUESTION NUMBERING (new): The Checklist Questions have no explicit sequence number in the database — they're ordered by however they appear on the Review Form's Checklist section (i.e., the same order ChecklistSection.tsx displays them in, likely tied to the load-order from dbo.[04_TEMP_02_Sample Checklists] or insertion order in 02_CORE_08_Checklists). 
   - Determine the correct "natural order" for a given Sample ID's questions — investigate READ-ONLY first: what column/mechanism determines display order in ChecklistSection.tsx today (an ID, a sort column, or just row insertion order)?
   - Generate a sequence number (1, 2, 3...) per question in that natural order, and prefix it to the question text wherever it's displayed — e.g. "1. Was the field exam completed timely?" — in BOTH the new summary table and the existing detail table.

2. NEW SUMMARY TABLE (before the detail table): Columns: CHECKLIST QUESTION (numbered, ascending = natural Review Form order) | COUNT | EXPOSURE.
   - Scope: this report is effectively single-Sample-ID by design (Geoff confirmed usage is almost exclusively within one Sample). Build the summary table over whatever set of reviews/questions the applied filters return — do not add special-casing for "single sample" since the existing filters already naturally constrain it; just aggregate over the filtered result set.
   - COUNT = number of borrowers/reviews with a "No" response to that question.
   - EXPOSURE = combined committed exposure of those "No"-response borrowers for that question (reuse the existing Commitment-join pattern already used elsewhere in this report's backend, if not already present — confirm).
   - NO totals row (Geoff explicitly declined one).

3. DETAIL TABLE — filter + group (update existing table): 
   - Group by Borrower Name (Review ID), one block per customer.
   - Include ONLY questions with a "No" response for that borrower — a borrower whose answers are all "Yes"/"N/A" is excluded entirely (no empty block for them).
   - Apply the same question-numbering prefix here too.
   - Keep the existing columns (Category, Question, Response, Comments minus Section/Guidance, as already fixed) — just add the number prefix and the filter/group behavior.

Investigate the question-ordering mechanism first (item 1) and report what you find before implementing — I want to confirm the "natural order" source is correct before it's baked into both tables. Then implement all three pieces together. Show diffs (backend + frontend). Rebuild, run tests. Do NOT commit.
