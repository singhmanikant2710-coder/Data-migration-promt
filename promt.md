Confirmed: slowness is Azure SQL MI network latency, not our code (stripHtml:false was still slow past 228s, and every trivial SELECT TOP(1) takes ~200ms). Bug 222 has no regression; deployed will be fast.

Revert the TEMPORARY diagnostic: set stripHtml back to true in ExportReviews (remove the "TEMPORARY DIAGNOSTIC" comment too). Keep StripHtml + BOM. Rebuild to confirm it compiles. Do NOT touch the .accdb or tsbuildinfo. Do NOT commit.
