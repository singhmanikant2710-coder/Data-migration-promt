READ-ONLY. Do NOT edit. Critical: confirm where the Initial Memo PDF is generated.

The network tab shows the memo download triggers a fetch to an "initial-memo"
endpoint (initiator api.ts:280), returning ~511 kB. This strongly suggests the
PDF is generated SERVER-SIDE, not in the browser via ReviewPDFModal.

Report only, no edits:

1. Open frontend/src/services/api (or wherever api.ts is) around line 280. What
   does the "initial-memo" call do? Show the endpoint URL it fetches.

2. Find the SERVER/API route that handles "initial-memo". Is it a Next.js API
   route (e.g. app/api/.../initial-memo/route.ts) or a backend .NET endpoint?
   Show the file. Does it call pdf(...).toBuffer()/renderToStream() with
   @react-pdf/renderer ON THE SERVER to produce the PDF?

3. If the PDF is generated server-side:
   - Confirm ReviewPDFModal's client-side pdf().toBlob() is NOT the path used for
     the actual downloaded Initial Memo (maybe it's only used elsewhere).
   - Identify which PDF component the server renders (InitialMemoPDF? ReviewPDF?).
   - Explain how fonts must be registered server-side: NOT window.location.origin
     (no window on server) and NOT a browser URL — it needs either an absolute
     filesystem path to the .ttf, or a fetched/bundled font buffer, registered
     before pdf().toBuffer() runs on the server.

4. Tell me definitively: for the Initial Memo the user downloads, is the PDF
   built on the CLIENT or the SERVER, and in which exact file must the font be
   registered for THAT path?

Output: the api.ts call + the server route + client-vs-server verdict + where
fonts must be registered server-side. No edits.
