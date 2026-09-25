Decisions on your 3 flags:
1. Bump payloadVersion "v19" -> "v20" (BlackbookSummaryService.cs:58)
   so QA doesn't see stale cached payloads after deploy.
2. MonthSummaryTable.tsx:1346 — covenant with no actual: return null
   (blank) instead of 0, same as 6a. Covenant group only.
3. 6f-B — when no slot matches, also set Threshold = null (don't
   borrow the unmatched definition's threshold for the violation
   highlight).
Same rules: build, tests, do not commit.
