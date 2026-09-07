Index check done. Now apply the fix: the ORDER BY I added is the cause of the 283s slowdown (deployed has no ORDER BY and is fast). 

FILE: ExportsController.cs, ExportReviews SQL:
Revert to the original fast query — remove ORDER BY, keep WITH (NOLOCK) as it originally was:
   const string sql = "SELECT * FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK);";

KEEP the real Bug 222 fixes only:
- StripHtml on string cells (with its fast-path + whitespace normalization) — strips HTML and collapses embedded newlines (this is what fixes CSV row-splitting; ORDER BY was never needed for that).
- The UTF-8 BOM.
- stripHtml: true from ExportReviews.

Also DELETE the temp file c:/Users/CC438/AppData/Local/Temp/strip222/equiv.ps1 — it must NOT be part of the changes/commit. Only ExportsController.cs should change.

Rebuild (dotnet build). Do NOT commit. I'll time the download — it should be fast again like deployed.
