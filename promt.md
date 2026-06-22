DIAGNOSTIC ONLY — DO NOT EDIT ANY FILE. Read the files below and report back. Make zero changes.

Read these files:
1. frontend/src/app/review-queue/page.tsx
2. frontend/src/app/review-history/page.tsx
3. frontend/src/services/api/reviewHistory.ts

Report the following with exact code snippets + line numbers:

From review-queue/page.tsx:
- The exact JSX of the Borrower Name cell (the clickable link).
- How navigation is done (Link href / router.push / onClick) and the EXACT destination route string with all query params (section, borrower, reviewId, sampleId).
- The exact JSX of the Linesheet icon/link and how its click works (download vs open) + which field/URL it uses.
- The row object's type name and which fields it reads (e.g. row.reviewId, row.sampleId, row.borrowerName).

From review-history/page.tsx:
- The exact JSX of the current Borrower Name cell (currently plain text).
- The exact JSX of the Linesheet icon if present.
- The row object type used in the table .map(), and list ALL fields available on each row.

From reviewHistory.ts:
- The TypeScript interface/type of a single row returned by the API — list EVERY field.
- Explicitly confirm whether reviewId and sampleId fields exist on that type.

Do NOT modify any file. If a path is not found, report it and stop.
