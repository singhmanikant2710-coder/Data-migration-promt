Bug 222 — export still ~281s even with the NEW binary (restarted multiple times, ORDER BY already removed). So ORDER BY was NOT the cause. Isolate the real bottleneck now.

1. Temporarily change ExportReviews to pass stripHtml: false. Rebuild, restart the API, and time one download.
   - If FAST (seconds) → StripHtml is the bottleneck. The fast-path isn't helping, likely because the large rich-text cells DO contain '<' (base64 <img>), forcing the full regex + HtmlDecode path on huge strings. We'll then optimize (e.g. cap/skip cells over a size threshold, or strip images by index-scan not regex).
   - If STILL ~281s → StripHtml is NOT the cause. Move to step 2.

2. If not StripHtml, check these (report findings, don't fix yet):
   a. Is the local backend connecting to a REMOTE database? Print the Server/Data Source from the connection (redact credentials). Pulling ~150k rows with nvarchar(max) base64 columns over a network is a prime suspect for slow-local / fast-deployed.
   b. ms.ToArray() — is the whole CSV buffered into a MemoryStream then .ToArray()'d (duplicating hundreds of MB on the LOH) before sending? 
   c. ~18M await writer.WriteAsync calls (150k rows × ~120 cols × 2) — each an async state machine.

Report: (1) timing with stripHtml:false, (2) if still slow, the DB server location + whether ms.ToArray buffers everything. This will pinpoint the real cause. Do NOT commit.
