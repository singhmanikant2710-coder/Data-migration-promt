Bug 220 — apply Geoff's two refinements to NonCompliantCovenantsPDF.tsx. Show diffs, rebuild, do NOT commit. Do NOT touch routing, backend, predicate, or IsPerformanceCategory (all working).

1. HEADER top-right: currently shows the sample caption ("354 - 4/1/2026 - Continuous Review - Corporate Segments"). Replace it with the DOWNLOAD DATE/TIME, matching other CRM reports (CrmFindingsObservationsPDF / ScorecardResultsPDF / CrmSummaryTablePDF). Find how those reports render today's date-time in the header-right and mirror that exact format. Remove reportingCaption from the header.

2. TEXT WRAPPING in the details table: Status and other text columns are cramped. Ensure Status, Category, Covenant Type, and Customer Name cells wrap cleanly within their fixed column widths (no clipping, no overflow into adjacent columns). Do NOT use wordBreak (unsupported in @react-pdf v4) — use natural word-wrap within fixed width. Consider slightly rebalancing the fixed column widths if Status is too narrow for "Not-Compliant" to sit on one line, but keep total = CONTENT_WIDTH.

Show diffs. Rebuild. Do NOT commit.
