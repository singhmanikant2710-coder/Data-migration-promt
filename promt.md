Frontend only. File: frontend/src/app/review/[ecif]/review-info/components/sections/CrmRatingsSection.tsx
Single edit. Do not plan. Just apply.

In handleOpenRatingsTip, the help-tip lookup uses the wrong topic string. The DB row for this button is Help_tip_id 12, Help_tip_topic = "Unsatisfactory Ratings", but the code passes topic = "CRM Ratings", so no row matches and the dialog shows "No help tip available".

Change the topic constant from:
  const topic = "CRM Ratings";
to:
  const topic = "Unsatisfactory Ratings";

Leave the form constant ("04_REVIEW FORM"), the three-step fetch fallback, and everything else exactly as it is.

Note: the topic must match the DB Help_tip_topic value exactly (case and spacing). Do not add trimming or lowercasing to the primary exact-match call.
