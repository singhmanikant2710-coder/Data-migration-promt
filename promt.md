Single-file edit: frontend/src/components/pdf/CrmSummaryPDF.tsx

Two text/font-only changes. No layout, no widths, no wrapping — completely safe.

CHANGE 1: The "CRM Summary" header title has fontSize 20. Change to 14.
Find styles.headerTitle fontSize: 20, change to fontSize: 14.
Keep the right-side run date unchanged.

CHANGE 2: Rename the filter heading text "Current Filter Payload" to "Applied 
Report Filters" (all occurrences — likely the no-items page and the final 
appended page). Change ONLY the heading text string. Keep the filter data 
below unchanged.

CONSTRAINTS:
- ONLY: headerTitle fontSize 20 -> 14, and "Current Filter Payload" -> "Applied 
  Report Filters" (text only).
- Do NOT touch widths, alignment, wrapping, overflow, lineHeight, or anything 
  else.
- Only edit this one file. Show both changes.
