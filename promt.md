Frontend only. Single file: frontend/src/app/review/[ecif]/review-info/components/sections/CustomerInfoSection.tsx
Do NOT modify SelectField, ui.tsx, or any shared component. Do not plan. Just apply.

Issue: The Relationship Manager dropdown now loads Data Mart options formatted "Number - Name" (e.g. "48191 - LEO MUTCHLER"), but the saved value from the DB is only the name ("LEO MUTCHLER"), because the name and number are stored in separate columns. As a result ensureIncludesSelected() appends a duplicate bare-name entry to the list instead of matching the existing "Number - Name" option. Same for Portfolio Manager.

Fix: compute a display value that matches the option format, using the saved name plus the saved number when both are available.

1) Check whether the review payload already exposes the manager numbers to the frontend (e.g. r?.relationshipManagerNumber / r?.portfolioManagerNumber, or similar). Report what you find.

2) If the numbers ARE available in the payload:
   Build the display value as `${number} - ${name}` when the number exists, otherwise fall back to the bare name. Pass that computed value as the SelectField `value` and into ensureIncludesSelected, for both Relationship Manager and Portfolio Manager.

3) If the numbers are NOT available in the payload:
   Do not guess. Stop and report that the backend read path (the review GET response) needs to also return Relationship_mgr_number and Portfolio_mgr_number before this can be fixed.

Change nothing else on the page.
