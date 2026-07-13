Apply this fix now. Single file only.

File: backend/src/Casrr.Api/Controllers/ReviewController.cs

Problem: the save endpoint returns 400 "No changes were provided" even though the request body contains a valid scorecard section:
{ "ecif": "id-17836", "reviewId": 17836, "sampleId": 311, "scorecard": { "change": "Upsert", "data": { "comments": "<b>Testing </b>Scorecard Comments" } } }

Cause: there is a validation/guard in the Save endpoint that counts how many sections were supplied (to reject empty requests). That check does not include dto.Scorecard, so a scorecard-only save is treated as "no changes".

Find that validation/guard and add Scorecard to it — exactly the same way RiskRatingJustification is counted there.

Do not change any other validation logic or any other section's behaviour.

Apply and show me the diff. Do not touch any other file.
