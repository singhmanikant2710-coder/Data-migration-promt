Fix both compile errors now.

ERROR 1 — backend/src/Casrr.Api/Controllers/ReviewController.cs (~line 152):
  CS8852: Init-only property 'ReviewFormSaveRequest.PostedSections' can only be assigned in an object initializer.
  The line `try { dto!.PostedSections = postedSections.ToArray(); } catch {}` fails because PostedSections is { get; init; }.
  Fix: change PostedSections from `init` to `set` in ReviewFormSaveModels.cs, OR (preferred) avoid mutating dto and pass postedSections through however the existing code already echoes posted sections back. Pick whichever is less invasive and does not alter existing behaviour — tell me which you chose.

ERROR 2 — frontend page.tsx (~line 529):
  ts(2339): Property 'scorecard' does not exist on type 'ReviewFormSaveRequest'.
  Fix: add an optional `scorecard` property to the frontend TypeScript type ReviewFormSaveRequest (in frontend/src/services/api/reviews.ts or wherever that type is declared), matching the shape of the other single-object sections, e.g.:
    scorecard?: { change: string; data: any };

Apply both fixes and show me the diffs. Do not change any other behaviour.
