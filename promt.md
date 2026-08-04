Single-file edit: frontend/src/components/pdf/HtmlRichText.tsx
[decodeEntities fix — &amp;->&, &gt;->>, &lt;-><, etc., &amp; LAST]
(same prompt as given earlier for Issue 2)

Single-file edit (or shared util): frontend/src/components/pdf/ReviewPDF.tsx 
(CAS Linesheet)

CONTEXT: Some non-narrative fields in the CAS Linesheet render HTML entities 
literally ("&gt;= 1.10x", "&amp;", "&lt;") because they are plain <Text> 
(not routed through HtmlRichText), or use a stripHtml helper that doesn't 
decode entities.

Two specific places from diagnostics:
1. The stripHtml helper (used for Policy Exception Comments) strips tags and 
   replaces &nbsp; but does NOT decode &lt;, &gt;, &amp;.
2. Non-narrative table cells (e.g. covenant thresholds like "&gt;= 1.10x") 
   render raw entity-encoded strings.

FIX:
a) Update the stripHtml helper in ReviewPDF.tsx to ALSO decode the named 
   entities after stripping tags. Add these decode steps (decode &amp; LAST):
     .replace(/&nbsp;/gi, " ")
     .replace(/&lt;/gi, "<")
     .replace(/&gt;/gi, ">")
     .replace(/&quot;/gi, "\"")
     .replace(/&#39;/gi, "'")
     .replace(/&apos;/gi, "'")
     .replace(/&#(\d+);/g, (_, n) => String.fromCharCode(parseInt(n, 10)))
     .replace(/&#x([0-9a-f]+);/gi, (_, n) => String.fromCharCode(parseInt(n, 16)))
     .replace(/&amp;/gi, "&")

b) For non-narrative table cell values that may be entity-encoded (e.g. 
   covenant threshold strings), apply the same decode. If there is a common 
   helper (like `out()` or a text formatter) used for these cells, prefer 
   decoding there. Otherwise, create a small local `decodeEntities` helper 
   in ReviewPDF.tsx and wrap the specific entity-prone cell values (covenant 
   threshold / covenant text fields) with it.

Report which specific cell/field values you wrapped, and confirm stripHtml 
now decodes entities.

CONSTRAINTS:
- Do NOT touch pageSetup.ts, page layout, or unrelated fields.
- Only decode entities; do not change the actual data or other formatting.
- Show the updated stripHtml and any cell values you wrapped.


Single-file edit: frontend/src/components/pdf/ReviewPDF.tsx (CAS Linesheet)

DATA ERROR (Geoff): The "Breaches Addressed" field in the Covenant and 
Monitoring Information table shows wrong/blank data. It should display 
[Covenant_breaches_addressed] from the CORE Reviews table.

ROOT CAUSE (confirmed): The backend already provides this value at 
data.covenants.info.breachesMitigated (mapped from 
Reviews.Covenant_breaches_addressed in SqlReviewRepository.GetCovenantsSectionAsync). 
But the Linesheet currently reads it from data.reviewInfo.* variants 
(Covenant_breaches_addressed / covenantBreachesAddressed etc.), which are 
NOT populated — hence blank/incorrect.

Current code:
  const covBreaches = stripHtml(
    ((ri as any)?.Covenant_breaches_addressed ??
     (ri as any)?.covenant_breaches_addressed ??
     (ri as any)?.CovenantBreachesAddressed ??
     (ri as any)?.covenantBreachesAddressed ?? "") as any
  );

FIX: Bind covBreaches to the correct source — data.covenants.info.breachesMitigated 
(the backend-provided, correctly-populated field). Keep stripHtml for safety:

  const covBreaches = stripHtml(
    ((data as any)?.covenants?.info?.breachesMitigated ?? "") as any
  );

CONSTRAINTS:
- Do NOT touch the backend or any query (it already returns the correct value).
- Only change the frontend binding for covBreaches to read from 
  data.covenants.info.breachesMitigated.
- Keep the existing stripHtml wrapper and the "-" fallback in the render.
- Do NOT touch other fields or layout.
- Only edit this one file. Show the updated covBreaches line.




Single-file edit: frontend/src/components/pdf/ReviewPDF.tsx (this is the CAS 
Linesheet component — the filename is ReviewPDF but it renders the CAS 
Linesheet report).

Make EXACTLY ONE change. Do NOT search for or wrap multiple cells. Do NOT 
loop or repeat.

The `stripHtml` helper function in this file strips HTML tags and replaces 
&nbsp;, but does NOT decode &lt;, &gt;, &amp;. Update ONLY the stripHtml 
function to also decode these named entities (decode &amp; LAST).

Find the stripHtml function and add entity decoding to its return chain. 
The final stripHtml should decode entities like this (keep its existing 
tag-stripping logic, just add the entity decodes):

  .replace(/&nbsp;/gi, " ")
  .replace(/&lt;/gi, "<")
  .replace(/&gt;/gi, ">")
  .replace(/&quot;/gi, "\"")
  .replace(/&#39;/gi, "'")
  .replace(/&apos;/gi, "'")
  .replace(/&#(\d+);/g, (_, n) => String.fromCharCode(parseInt(n, 10)))
  .replace(/&#x([0-9a-f]+);/gi, (_, n) => String.fromCharCode(parseInt(n, 16)))
  .replace(/&amp;/gi, "&")

CONSTRAINTS:
- Change ONLY the stripHtml function. Nothing else.
- Do NOT wrap any table cells, do NOT touch other fields, do NOT loop.
- This is a single, one-time edit to one function.
- Show the final stripHtml function once, and stop.
