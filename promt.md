Apply this edit now. Single file only.

File: frontend/src/app/review/[ecif]/review-info/components/sections/ScorecardsSection.tsx

Add a new SectionCard titled exactly "Scorecard Comments" BELOW the existing Scorecards grid.

Inside it, use the existing RichTextEditor component (from @/components/ui/RichTextEditor), wired exactly like RiskRatingJustificationSection.tsx does:

  value = response.form.scorecard?.comments ?? ""
  onChange = (html) => { if (isEditing && changes) changes.setSection("scorecard", { comments: html }); }
  readOnly={!isEditing}
  showToolbar={isEditing}
  minHeight={220}
  placeholder="Add comments about the scorecards…"
  ariaLabel="Scorecard Comments"

Reuse the existing SectionCard and RichTextEditor — no new UI patterns.

DO NOT touch the existing Scorecards grid logic or its stagePatchForGroup / setSection("transactions", ...) cascade.

Also confirm: is "scorecard" present in the SectionKey union type in FormChangesContext.tsx? If not, add it.

Apply and show me the diff. Do not touch any other file.
