Single-file edit: frontend/src/components/pdf/HtmlRichText.tsx

BUG: The decodeEntities function does not decode named HTML entities. The 
current code has no-op replacements (replacing "&" with "&", "<" with "<", 
etc.) instead of decoding the entity forms. So "&amp;" renders literally as 
"&amp;" in the PDF (seen in Initial/Final Memo narrative text: 
"Excluding net income &amp; distributions").

Current (broken) decodeEntities:
  function decodeEntities(input: string): string {
    return input
      .replace(/&nbsp;/gi, " ")
      .replace(/&/gi, "&")        // no-op
      .replace(/</gi, "<")        // no-op
      .replace(/>/gi, ">")        // no-op
      .replace(/"/gi, "\"")       // no-op
      .replace(/'/gi, "'")        // no-op
      .replace(/&#39;/gi, "'")
      .replace(/&#(\d+);/g, (_, n) => String.fromCharCode(parseInt(n, 10)))
      .replace(/&#x([0-9a-f]+);/gi, (_, n) => String.fromCharCode(parseInt(n, 16)));
  }

FIX: Replace the no-op named-entity lines with correct entity decoding. 
IMPORTANT: decode &amp; LAST (after all other named entities) so that 
entities like "&amp;lt;" don't get partially decoded incorrectly. Correct 
version:

  function decodeEntities(input: string): string {
    return input
      .replace(/&nbsp;/gi, " ")
      .replace(/&lt;/gi, "<")
      .replace(/&gt;/gi, ">")
      .replace(/&quot;/gi, "\"")
      .replace(/&#39;/gi, "'")
      .replace(/&apos;/gi, "'")
      .replace(/&#(\d+);/g, (_, n) => String.fromCharCode(parseInt(n, 10)))
      .replace(/&#x([0-9a-f]+);/gi, (_, n) => String.fromCharCode(parseInt(n, 16)))
      .replace(/&amp;/gi, "&");   // MUST be last
  }

Key points:
- &amp; -> & must run LAST (after &lt;, &gt;, etc.) to correctly handle 
  double-encoded sequences.
- Decode &lt; -> <, &gt; -> >, &quot; -> ", &#39;/&apos; -> ', plus numeric 
  and hex entities (keep those as they already work).

CONSTRAINTS:
- Only fix the decodeEntities function in HtmlRichText.tsx.
- Do NOT change any other logic in HtmlRichText, and do NOT touch 
  InitialMemoPDF.tsx / FinalMemoPDF.tsx for this issue (the fix is central).
- Do NOT touch pageSetup.ts or page layout.
- Only edit this one file. Show the final decodeEntities function.
