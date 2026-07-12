SELECT COLUMN_NAME, DATA_TYPE, CHARACTER_MAXIMUM_LENGTH, IS_NULLABLE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA = 'dbo'
  AND TABLE_NAME = '02_CORE_02_Reviews'
  AND COLUMN_NAME = 'Scorecard_information';



  Approved — proceed. The [Scorecard_information] column exists on dbo.[02_CORE_02_Reviews]. Use LIVE DB, ignore columns.csv.

Implement in this ORDER, and STOP after each stage for my confirmation and testing:

STAGE 1 — BACKEND (read path only):
- Add Scorecard_information to ReviewEntity, the review GET DTO, the repository SELECT, and the service mapping, mirroring exactly how Risk_rating_justification is handled today (same null-safety, same NVARCHAR/HTML treatment).
- Do NOT touch the save path yet.
- After this stage I will restart the API and verify the field appears in the GET response.

STAGE 2 — BACKEND (save path):
- Add Scorecard_information to the Save DTO, the repository UPDATE, and the service mapping, again mirroring Risk_rating_justification.
- Additive only — no existing field's behaviour may change.

STAGE 3 — FRONTEND:
- Surface the value in the review payload and expose it (mirroring how riskRatingJustification is exposed).
- Add a "Scorecard Comments" SectionCard to ScorecardsSection.tsx with the existing RichTextEditor, wired exactly like RiskRatingJustificationSection (value, onChange staging to FormChangesContext, readOnly={!isEditing}, showToolbar={isEditing}, minHeight 220).
- Place it BELOW the scorecards grid.
- Header text exactly: "Scorecard Comments".

Hard constraints:
- Do NOT modify the existing ScorecardsSection group/transaction staging logic — the new field is independent and review-level.
- Reuse the existing RichTextEditor and SectionCard — no new UI patterns.
- Confirm the FormChangesContext section key you use for the new field and that the save aggregator actually picks it up and sends it (this is the step most likely to be missed — verify it end-to-end).

Show me the diff for STAGE 1 and STOP.
