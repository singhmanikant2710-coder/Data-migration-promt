READ-ONLY. Find where Internal/External Notes are displayed (UI and PDF) so we can decode HTML entities on display. Quote with paths.

CONTEXT: In tblCustomer.strInternalNotes, legacy/migrated data contains HTML entities (&quot;, &amp;, &lt;, &gt;) instead of raw characters (" & < >). New saves store raw characters correctly. So the fix is to DECODE entities when displaying notes (not change the DB). Need all display points.

1) Where are Internal Notes and External Notes displayed in the UI? Search for strInternalNotes, InternalNotes, "Internal Notes". Quote each render location (blackbook edit, customer edit/view, etc.) and how the value is shown (textarea value, div text, etc.).
2) Where are notes rendered in the PDF? Search BlackBookPdf.tsx (and any PDF component) for notes. Quote how notes text is placed into the PDF.
3) Is there any existing HTML-decode/escape helper in the codebase (decodeHtmlEntities, he.decode, unescape, etc.)? Quote it if it exists — we can reuse it.
4) How is the notes value fetched (which API returns strInternalNotes)? Is it raw from DB (so entities come through as-is)? Quote the fetch/type.

OUTPUT:
- A) All UI display points for Internal/External Notes, quoted.
- B) PDF render point for notes, quoted.
- C) Any existing decode helper, quoted (or "none — need to add").
- D) Exact fix locations: where to apply a decode-entities helper on the notes value before display (UI + PDF).
- No fix. Findings only.
