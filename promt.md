Bug 218 — CRM Policy Exceptions report: change the header date/time (top-right) text color to white. SINGLE FILE. Show diff, do NOT commit.

FILE: frontend/src/components/pdf/PolicyExceptionsPDF.tsx (the CRM Policy Exceptions report — confirm this is the correct file; if the report is named differently in the codebase, find the right one)

Find the header's right-side date/time <Text> (the one added earlier in Bug 218 for the header showing download date/time, replacing the old sample caption). Change its color to white (e.g. color: "#FFFFFF" or the colors.white token from pageSetup.ts if one exists — check and reuse if available).

Do NOT change the header background/banner color, font size, position, or any other styling — only the text color of that one date field.

Show diff. Rebuild. Do NOT commit.
