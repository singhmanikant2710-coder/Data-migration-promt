Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break.

TASK (UAT #193 + #194, Geoff) — Review Form rich text fields → PDF output:

#193 Tables: Tables created in a Review Form rich text field (e.g. Risk
Rating Justification), or pasted from Excel/Word, are stretched across the
full page width in the PDF. Geoff asks: can table and column widths be
adjusted in the editor and kept in the PDF? Example: a 2-column Excel table
(label + amount, e.g. "Total A/R Balance | $25,811,623") is narrow in Excel
but full-width in the PDF.

#194 Images: Images pasted into a rich text field (e.g. a screenshot of a
small financial table in Risk Rating Justification) appear larger in the PDF
than in the Review Form. Geoff wants pasted images to keep their pasted size.

Expected:
- PDF honours the table/column widths that exist in the saved HTML
  (editor-set widths, <colgroup>/<col>, width attrs, style width in px/pt/%
  from Excel/Word paste), and honours image width/height (attrs, style, or
  editor resize).
- Units: convert px → pt at 0.75; pt as-is; % of available content width.
- Never overflow the page/column: if a table or image is wider than the
  available width, scale it down proportionally (images keep aspect ratio).
- Image with no size info: render at its natural pixel size × 0.75 pt,
  capped at available width (read dimensions from the data URI header —
  PNG/JPEG/GIF — no new dependency). If natural size can't be read, keep
  today's behaviour.
- Table with NO width info anywhere: keep today's rendering byte-identical
  (do not change old saved data without width info).
- If the editor library supports column resize / image resize via existing
  configuration only (no new npm package), enable it so users can adjust
  widths; if it needs a new package or a big change, STOP and ask me.

Work in 4 phases:

PHASE 1: READ-ONLY DISCOVERY (no edits)
- Trace end to end with file:line: rich text editor component (library +
  config: table, paste, image handling), what HTML is actually saved for
  (a) a table built in the editor, (b) a table pasted from Excel, (c) from
  Word, (d) a pasted image — show a real sample of each from the DB or
  editor output.
- Check whether anything strips width/style/colgroup/width/height attrs on
  the way: editor paste rules, frontend sanitizer, API/backend HTML
  sanitizer, DB column length. If widths never reach the DB, say so.
- In the PDF: HtmlRichText.tsx (and anything it calls) — how tables, cells
  and images are laid out today (file:line), and where full width comes from.
- List EVERY caller of HtmlRichText / the rich-text-to-PDF path: all 14
  reports, Initial/Final Memo, CAS Linesheet, email body (if HTML), any
  on-screen preview. Note the available width in each (portrait/landscape,
  table cells, columns).
- Report existing bugs/risks; don't fix outside this task.
- If ambiguous or a business decision is needed, STOP and ask me with
  A/B/C options + trade-offs.

PHASE 2: PLAN
- Smallest correct, generic change in the shared rich-text renderer, driven
  by the HTML itself — no per-report or per-field special cases, no change
  to pageSetup.ts shared defaults.
- Width calculation in one small pure helper (parse px/pt/%, column widths,
  scale-to-fit, image header dimensions) with unit-testable functions.
- No schema/DB change. If a backend sanitizer must keep width/style attrs,
  allow ONLY the needed width/height attributes and width/height style
  properties — nothing else (no new XSS surface). Stop and ask before
  changing sanitizer behaviour.

PHASE 3: IMPLEMENT
- Touch only what this task needs. Match each file's existing style.
- Existing PDFs for content without width info must be byte-identical in
  layout.
- Clean up any temp scripts/harness files you create.

PHASE 4: VERIFY + REPORT
- Build backend and frontend (report file-lock errors separately from real
  compile errors).
- Verify with real functions and real saved HTML samples (Excel table, Word
  table, editor table, pasted screenshot, large image, small image).

Report:
1. Root cause / what you found (file:line) for #193 and #194 separately,
   plus any other issues noticed (incl. whether widths are stripped)
2. ADDED lines (file:line)
3. REMOVED / CHANGED lines (file:line)
4. On-screen + PDF behaviour: before vs after (editor, each report, memos)
5. NULL / empty / edge cases and the result for each: field NULL/empty;
   table with no widths; widths only on some columns; widths summing over
   100% or wider than page; % widths; merged cells (colspan/rowspan);
   nested table; table inside a narrow report column; image without
   width/height; image wider than page; tiny image; unreadable/corrupt
   image data; external image URL; landscape vs portrait report
6. Other screens/reports checked and confirmed unaffected
7. Build results
8. git status: which files changed (flag artifacts that are not yours)
Do not commit or push.
