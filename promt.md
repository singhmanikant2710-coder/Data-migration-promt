Follow-up 2 for UAT #194 (same task, do not commit):

The previous fix did not solve the real case. After restarting the dev server, the image is still missing from the CAS Linesheet PDF. The REAL saved HTML (from the live DOM) is:

<p class="MsoNormal"><o:p>&nbsp;<img src="data:image/png;base64,..." style="max-width:100%;height:auto;display:block;" alt=""></o:p></p>

Your fixtures used <div><o:p>...</o:p></div>; the real parent is <p>. An image pasted elsewhere (not inside <p>) renders fine, so sizing works — the loss is an <img> inside a <p> (and possibly other inline-rendered containers).

Phase 1 (read-only): with file:line, trace this exact HTML through flattenTransparentNodes → the <p> branch → renderInlineNodesAst, and show where the <img> is dropped. Then list EVERY container whose children go through inline rendering (p, span, a, strong/b, em/i, u, li, td/th, h1/h2, blockquote, MsoNormal-style p, etc.) and whether an <img> inside each survives today. Explain whether partitionInlineAndImagesAst (previously reported as dead code) was meant for this.

Fix (generic, smallest change):
- Any <img> appearing inside an inline-rendered container must render, in document order: split the children into inline-text runs and image blocks (reuse partitionInlineAndImagesAst if suitable, otherwise one small helper), keep the container's existing text styles for the text runs, and render images with the existing computeImageLayout sizing.
- A run that is only whitespace/&nbsp; next to an image must not add an extra blank line.
- HTML without images inside such containers must render byte-identically to today.
- No editor, backend or DB change. No new URL schemes.

Verify against the EXACT real HTML above plus: <p>text <img> more text</p>; <p><img></p>; <li>item <img></li>; <td><img></td> (Excel/Word table cell); <p><span><img></span></p>; <p><strong>x</strong><img></p>; nested <o:p> inside <p> inside <div>. Use the real shipping functions.

Report:
1. Root cause (file:line) and why the previous fixture missed it
2. ADDED / CHANGED / REMOVED lines (file:line)
3. Before vs after for each case above
4. Build results (tsc + npm run build)
5. git status
Do not commit or push.
