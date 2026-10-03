Follow-up for UAT #194 (same task, do not commit):

Real-data finding: a pasted image (src="data:image/png;base64,...", style="max-width:100%;height:auto;display:block;") is visible in the Review Form but missing from the CAS Linesheet PDF. Its saved parent is a Word namespaced tag:
'<o:p>&nbsp;<img src="data:image/png;base64,..." style="max-width:100%;height:auto;display:block;" alt=""></o:p>'
It came from pasting right after a table pasted from Word. Excel/Word tables themselves render correctly.

Phase 1 (read-only): in HtmlRichText.tsx, show with file:line how the tokenizer/parser/renderer handle (a) tag names containing ":" (o:p, w:*, v:*, st1:*), (b) any unknown tag, (c) their children. Confirm this is why the image (and any text inside such tags) is dropped. Check whether text inside <o:p> is also lost today.

Fix (generic, smallest change):
- Tokenizer must accept namespaced tag names (letters, digits, ":", "-", "_") for open, close and self-closing tags.
- Unknown / namespaced tags are TRANSPARENT: render their children as if the wrapper were not there (inline by default). Do not invent block spacing for them.
- Keep dropping (with children) only non-content tags: script, style, head, title, meta, link, xml, and Office conditional comments (<!--[if ...]> ... <![endif]-->). Don't add any new allowed URL schemes.
- HTML with no unknown/namespaced tags must render byte-identically to today.
- No editor, backend or DB change.

Report:
1. Root cause (file:line)
2. ADDED / CHANGED / REMOVED lines (file:line)
3. Before vs after for: <o:p>&nbsp;<img data-uri></o:p>; <o:p>text</o:p>; <o:p>&nbsp;</o:p> (empty marker — must not add extra blank lines); nested <span><o:p>…</o:p></span>; <w:sdt>text</w:sdt>; <script>/<style> (still dropped); Office conditional comments; HTML with no such tags (identical)
4. Build results (tsc + npm run build)
5. git status
Do not commit or push.
