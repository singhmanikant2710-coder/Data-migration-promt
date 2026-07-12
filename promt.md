Complete UAT #86 fully now (all stages). LIVE DB confirmed: dbo.[02_CORE_02_Reviews].[Scorecard_information] exists, NVARCHAR. Ignore columns.csv.

The Domain model (ScorecardSection in ReviewForm, ReviewQueue.cs) is already done. Complete the rest:

BACKEND — READ:
- SqlReviewRepository: SELECT r.[Scorecard_information] and map into form.Scorecard.Comments, in BOTH GetReviewByEcifAsync and GetReviewByKeysAsync. Add it to the EXISTING review-header query that already selects Risk_rating_justification — do not add a separate round-trip.

BACKEND — SAVE:
- IReviewRepository: SaveScorecardInfoAsync(int reviewId, string? comments, CancellationToken ct)
- SqlReviewRepository: UPDATE dbo.[02_CORE_02_Reviews] SET [Scorecard_information] = @comments WHERE [Review_id] = @id
- ReviewFormSaveModels: add the save DTO
- IReviewService / ReviewService: add the service method
- API controller: accept a "scorecard" section in the save body and route to the service
Mirror the Risk_rating_justification save flow exactly. Additive only.

FRONTEND:
- ScorecardsSection.tsx: add a "Scorecard Comments" SectionCard BELOW the existing Scorecards grid.
- Reuse the existing RichTextEditor + SectionCard. Wire exactly like RiskRatingJustificationSection:
  value = response.form.scorecard?.comments ?? ""
  onChange = (html) => { if (isEditing && changes) changes.setSection("scorecard", { comments: html }); }
  readOnly={!isEditing}, showToolbar={isEditing}, minHeight={220}
- Header text exactly: "Scorecard Comments".

CRITICAL — verify end-to-end that the centralized save flow picks up the "scorecard" section from FormChangesContext and actually sends it in the save request. Confirm "scorecard" is in the SectionKey union type. State this confirmation explicitly.

Do NOT touch the existing ScorecardsSection group/transaction staging ("transactions" section key cascade).

Apply everything and show me the diffs. STOP after applying.
