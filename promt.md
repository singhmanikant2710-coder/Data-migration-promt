Approved — confirmed: dbo.[02_CORE_02_Reviews].[Scorecard_information] exists (NVARCHAR). Use the LIVE DB, ignore columns.csv.

Implement in this ORDER, and STOP after each stage for my confirmation and testing:

STAGE 1 — BACKEND READ PATH:
- Add the ScorecardSection to the Domain ReviewForm (Comments property).
- In SqlReviewRepository, SELECT [Scorecard_information] and map it into form.Scorecard.Comments, in BOTH GetReviewByEcifAsync and GetReviewByKeysAsync.
- Mirror exactly how Risk_rating_justification is read (same null-safety, same pattern).
- Do NOT touch the save path in this stage.
STOP. I will stop the running Casrr.Api, rebuild, restart, and verify via F12 that the review GET response now contains form.scorecard.comments.

STAGE 2 — BACKEND SAVE PATH:
- IReviewRepository: add SaveScorecardInfoAsync(int reviewId, string? comments, CancellationToken ct).
- SqlReviewRepository: implement the UPDATE on dbo.[02_CORE_02_Reviews].[Scorecard_information].
- ReviewFormSaveModels: add the save DTO.
- IReviewService / ReviewService: add the service method delegating to the repository.
- API controller: accept a "scorecard" section in the save request body and route it to the service.
- Mirror the Risk_rating_justification save flow exactly. Additive only — no existing field's behaviour may change.
STOP. I will rebuild and test that the save endpoint accepts the section.

STAGE 3 — FRONTEND:
- Add a "Scorecard Comments" SectionCard BELOW the existing Scorecards grid in ScorecardsSection.tsx.
- Reuse the existing RichTextEditor and SectionCard — no new UI patterns.
- Wire it exactly like RiskRatingJustificationSection: value from response.form.scorecard?.comments ?? "", onChange stages changes.setSection("scorecard", { comments: html }) when isEditing, readOnly={!isEditing}, showToolbar={isEditing}, minHeight 220.
- Header text exactly: "Scorecard Comments".
- CRITICAL: verify END-TO-END that the centralized save flow actually picks up the new "scorecard" section from FormChangesContext and sends it in the save request body. Also add "scorecard" to the SectionKey union type in FormChangesContext if it is not already there. This is the step most likely to be missed — confirm it explicitly.

Hard constraints:
- Do NOT modify the existing ScorecardsSection group/transaction staging logic (the "transactions" section key cascade) — the new comments field is independent and review-level.
- Follow the existing Clean Architecture layering.

Show me the diff for STAGE 1 and STOP.
