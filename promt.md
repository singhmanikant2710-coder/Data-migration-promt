Answers:

1. A – add dompurify (client-only).
   Reason: all real sinks are client-only (tipHtml is null during SSR), dompurify has zero transitive dependencies, is small, and is recognised by Veracode as a cleanser. isomorphic-dompurify pulls jsdom for no benefit, and a hand-rolled sanitizer may leave the flaws Open.
   Pin the exact version 3.4.16 (no ^ / ~) in package.json and update package-lock.json via npm install. No other dependency changes. In the report, note that package approval / npm feed availability must be confirmed with the bank before merge (we have raised this with the cloud team).

2. A – read test-data/rrj-20120.html via shell, read-only.
   Reason: verifying against real saved content (Excel/Word tables, <o:p>, data-URI images) is the strongest proof that normal content is unchanged. Use it only to run through the sanitizer and diff before/after. Never stage, modify, or copy it into the repo.

3. A – no code change in ReviewPDFModal.tsx (#59, #60, #61).
   Reason: there is no HTML sink – blobUrl is a blob: object URL created by URL.createObjectURL from the client-generated @react-pdf Blob, so no user/DB string reaches href/src. A blob: prefix check would not be recognised as a cleanser and would only add code to a file with no real flaw.
   Write ready-to-paste Veracode "Mitigate by Design" justifications for each of the three, in the same style as the already approved mitigations (explain the data source, why no untrusted input reaches the sink, same-origin, content type application/pdf).

Continue with PHASE 2 onward.
