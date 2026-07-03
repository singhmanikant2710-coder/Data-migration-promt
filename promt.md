Modify frontend files to add a reusable Help Tip component and use it in the CRM 
Ratings tab. The backend API GET /api/v1/help-tips?form=&topic= exists, and 
frontend/src/services/api/helpTips.ts has listLibrary(form, topic). 
Help tips are HTML content.

STEP 1 — Create a reusable component:
frontend/src/app/review/[ecif]/review-info/components/HelpTipIcon.tsx (new file)
- Props: { form: string; topic: string }
- On mount (or on first open), call listLibrary(form, topic) to fetch the tip.
- Render an (i) info icon. On hover/click, show a popover/tooltip displaying the 
  tip's HTML content (use dangerouslySetInnerHTML for the HTML, since tips are HTML).
- Handle loading/empty (if no tip found, show the icon but a simple "No help available" 
  or just the icon with nothing).
- Keep it small and self-contained.

STEP 2 — Use it in the CRM Ratings tab:
In CrmRatingsSection.tsx, replace the static InfoIconSquare (title="CRM Ratings...") 
in the section actions with:
  <HelpTipIcon form="04_REVIEW FORM_04" topic="Unsatisfactory Ratings" />

Modify ONLY: the new HelpTipIcon.tsx and CrmRatingsSection.tsx. 
If listLibrary or the API contract differs from what I described, adapt to the actual 
code in helpTips.ts. If something needs another file, STOP and tell me first.
