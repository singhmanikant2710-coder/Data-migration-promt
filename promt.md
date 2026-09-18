Context: .NET 8 Clean Architecture backend + Next.js/React/TypeScript frontend,
CASRR project. Two small visual tweaks to the "Review Summary for Management"
PDF component (the one we just built for CRM Summary for Management).

IMPORTANT: Only touch these two specific styling points in this component. Do not
change any table structure, data logic, or other report's styling.

1. Customer name font size: currently too large/prominent relative to the rest of
   the page (see attached screenshot — "198 MADISON AVE REALTY NY LLC" under
   "CUSTOMER NAME"). Reduce it to a size closer to the section headings (e.g.
   "RISK RATING JUSTIFICATION") rather than a large display heading — it should
   read as a bold label value, not a page title.

2. Header right-side text: currently shows date AND time (e.g. "9/18/2026,
   7:01:26 PM", white text on the navy bar). Change it to show DATE ONLY (e.g.
   "9/18/2026") — remove the time portion. Keep the white color and right
   alignment as-is.

Acceptance criteria:
- Customer name renders smaller/less dominant, consistent with the rest of the
  page's visual hierarchy.
- Header right side shows only the date (no time), still white, still
  right-aligned.
- No other part of this report or any other report is affected.
