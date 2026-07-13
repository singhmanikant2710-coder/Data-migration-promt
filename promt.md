Apply this edit now. Single file only.

File: frontend/src/app/review/[ecif]/review-info/page.tsx

Inside handleSave, alongside the other single-object section branches (next to the riskRatingJustification one), add:

if (s.scorecard) {
    payload.scorecard = {
        change: "Upsert",
        data: s.scorecard
    };
    count++;
}

Do not change any other branch or any other logic in handleSave.

Apply and show me the diff. Do not touch any other file.
