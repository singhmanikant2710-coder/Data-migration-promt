UAT #86 — Scorecards screen: add a "Scorecard Comments" Rich Text field.

Client requirement:
1. Add a Rich Text field to the Scorecards screen, similar to the existing comment fields in other sections of the Review Form.
2. Users enter comments about the scorecards they have assessed.
3. It must be bound to the DB field: [02_CORE_02_Reviews].[Scorecard_information]
4. UI/formatting must match the existing comment sections in the Review Form.
5. Section header must read: "Scorecard Comments"

Expected behavior: rich text editor displayed on the Scorecards screen; users can enter, edit and SAVE formatted comments; comments persist to Scorecard_information; appearance consistent with other comment sections.

YOUR TASK — report FIRST (read-only, no edits), then propose the plan:

1. Frontend:
   a. Show me ScorecardsSection.tsx as it exists today — its structure and how it currently saves (does it use FormChangesContext / setSection? What section key?).
   b. Find an existing rich-text comment section to copy from (e.g. the Finding Comments editor in CrmFindingsAndRatingsSection, or the UNSAT rationale editors in CrmRatingsSection, or Risk Rating Justification). Show the RichTextEditor component and exactly how it's wired (value, onChange, staging to FormChangesContext, and how the HTML is saved).

2. Backend — trace end-to-end what is needed for Scorecard_information:
   a. Does the column [Scorecard_information] already exist in dbo.[02_CORE_02_Reviews]? Verify against the LIVE DB (ignore columns.csv — it is stale). Give me the SQL to confirm.
   b. Does the review GET payload already return this field (check the DTO / repository SELECT)? If not, what must be added?
   c. Does the SAVE path already accept and persist it (check the save DTO / UPDATE statement)? If not, what must be added?
   d. Identify every file that needs changing on the backend (repository, service, DTO, controller).

3. Propose the implementation plan, file by file, in the ORDER we should apply it (backend first, then frontend), so I can test incrementally.

Constraints:
- Reuse the existing RichTextEditor component and the existing comment-section styling — do not build new UI patterns.
- Do not break any existing Scorecards behaviour or save logic.
- Follow the existing Clean Architecture layering used elsewhere in the backend.

Report findings and plan. STOP and wait for approval before any edits.
