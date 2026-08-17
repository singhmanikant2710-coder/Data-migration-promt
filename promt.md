Single-file edit: frontend/src/components/pdf/CrmSummaryPDF.tsx

Fix: The Scorecard ID value overflows the sc1 cell because it contains long unbroken segments separated by both "-" and "&", but soft breaks (\u200B) are only inserted after "-". Add a soft break after "&" as well, so the value wraps cleanly at every separator without changing any column width.

Exact change (before -> after):

BEFORE:
{out(row?.scorecardId).replace(/-/g, "-\u200B")}

AFTER:
{out(row?.scorecardId).replace(/([-&])/g, "$1\u200B")}

CONSTRAINTS:
- ONLY change this one .replace(...) expression on the scorecardId render line.
- Do NOT change sc1 width, td, tdText, or any style.
- The visible characters must stay identical — we are only inserting zero-width break points, not removing or altering "-" or "&".
- Do NOT touch any other file or any other cell.
- Only edit this one file. Show the diff.
