Bug 192 fix — Comments/rich-text cells leak raw HTML (<span style>, <div>, &nbsp;, &amp;) into PDFs because they bypass the (correct) HtmlRichText parser. Route them through it, and fix ReviewPDF's broken stripHtml. Render-side only for now. Show all diffs before applying. Do NOT touch HtmlRichText.tsx's parser logic (it's correct).

The existing pattern to mirror (already used for narrative fields): when a value contains markup, render via HtmlRichText with an out()/plain fallback:
   if (raw && /[<][^<>]*[>]/.test(raw)) {
     return <HtmlRichText html={raw} fallback={out(raw)} baseFontSize={...} />;
   }
   return <Text ...>{out(raw)}</Text>;

Apply this gating to the Comments cells that currently pass raw through out():
1. FinalMemoPDF.tsx — Findings comments (line ~995) and Observations comments (line ~1034). Wrap with the same HtmlRichText gate used by this file's narrative fields (lines ~894, ~955). Keep the existing cell <Text> style as the fallback path / wrapper.
2. InitialMemoPDF.tsx — Findings (line ~1115) and Observations (line ~1154). Same treatment, mirroring lines ~1012, ~1075.
3. CrmFindingsObservationsPDF.tsx — Comments cell (line ~531). Route through HtmlRichText with out() fallback.
4. ChecklistQuestionnairePDF.tsx — Comments cell (line ~257). Same.

5. ReviewPDF.tsx:
   (a) Findings comments (line ~698 source, rendered ~1272): apply the same HTML handling the other ~10 narrative fields use (lines ~720-766) so it's stripped/rendered, not raw.
   (b) Fix the broken stripHtml entity replacements (lines ~26-38): the five no-op identity replacements must decode real entities:
       &nbsp;→space, &lt;→<, &gt;→>, &quot;→", &#39;/&apos;→', &amp;→& (do &amp; LAST). Mirror the correct decodeEntities in HtmlRichText.tsx (lines ~85-95).

Notes:
- Prefer routing through HtmlRichText (preserves formatting) where a cell can host it; if a table cell can't host block content cleanly, use the module's stripHtmlToText-style plain-text strip instead — but do NOT leave raw out() on these HTML-bearing cells.
- Do NOT change the DB, the editor, or HtmlRichText's parser. This is render-side only.
- Also check quickly: is there a double-escaped case (stored as &lt;div&gt;)? If the gate regex won't catch escaped markup, ensure entity decoding still cleans it via the plain-text path.

List every file + line changed. Commit: "Fix Bug 192: route Comments/rich-text cells through HtmlRichText and fix ReviewPDF stripHtml entity decoding".
