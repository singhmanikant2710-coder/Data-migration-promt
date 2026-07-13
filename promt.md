NEW TASK — UAT #86: add a "Scorecard Comments" rich text field to the Scorecards screen.

Ignore all previous context. Fresh start.

DB: dbo.[02_CORE_02_Reviews].[Scorecard_information] EXISTS, type NVARCHAR. Use the LIVE DB, ignore columns.csv (it is stale).

ALREADY DONE: the Domain model — ScorecardSection was added to ReviewForm in ReviewQueue.cs. Do not redo it.

DO ALL OF THE FOLLOWING:

1) BACKEND — READ (SqlReviewRepository.cs):
   SELECT r.[Scorecard_information] and map it into form.Scorecard.Comments, in BOTH GetReviewByEcifAsync and GetReviewByKeysAsync. Add it to the EXISTING review-header query that already selects Risk_rating_justification — do not add a separate DB round-trip.

2) BACKEND — SAVE:
   - IReviewRepository.cs: add SaveScorecardInfoAsync(int reviewId, string? comments, CancellationToken ct)
   - SqlReviewRepository.cs: UPDATE dbo.[02_CORE_02_Reviews] SET [Scorecard_information] = @comments WHERE [Review_id] = @id
   - ReviewFormSaveModels.cs: add the save DTO
   - IReviewService.cs / ReviewService.cs: add the service method delegating to the repository
   - The review save controller: accept a "scorecard" section in the save body and route it to the service
   Mirror the Risk_rating_justification save flow EXACTLY. Additive only — no existing field's behaviour may change.

3) FRONTEND (ScorecardsSection.tsx):
   Add a "Scorecard Comments" SectionCard BELOW the existing Scorecards grid.
   Reuse the existing RichTextEditor and SectionCard components — no new UI patterns.
   Wire it exactly like RiskRatingJustificationSection:
     value = response.form.scorecard?.comments ?? ""
     onChange = (html) => { if (isEditing && changes) changes.setSection("scorecard", { comments: html }); }
     readOnly={!isEditing}
     showToolbar={isEditing}
     minHeight={220}
   Header text exactly: "Scorecard Comments"

CRITICAL: verify end-to-end that the centralized save flow picks up the "scorecard" section from FormChangesContext and actually SENDS it in the save request body. Confirm "scorecard" exists in the SectionKey union type in FormChangesContext. State this confirmation explicitly in your summary.

DO NOT touch the existing ScorecardsSection group/transaction staging (the "transactions" section key cascade).

Apply everything and show me the diffs. STOP after applying.
