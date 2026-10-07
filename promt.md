Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break. Veracode static scan casrr/api-20261005.3 (sandbox casrr/api-qa).
FRONTEND ONLY – do not touch any .NET code.

TASK: Veracode GROUP A – CWE-80 Basic XSS (14 Medium, status Open, no
mitigation). HTML from the API/DB is rendered without sanitization. Fix so
that script/event-handler content can never execute, while normal content
looks EXACTLY the same on screen and in the PDF.

Flaws (line numbers are from the 5 Oct scan – locate by file + surrounding
code, the code may have moved):
#48 ChecklistSection.tsx:200
#49 CollateralSection.tsx:245
#50 CovenantsSection.tsx:966
#51 CrmRatingsSection.tsx:285
#52 CustomerInfoSection.tsx:443
#53 KeyRisksSection.tsx:142
#54 PolicyExceptionsSection.tsx:694
#55 RegulatoryTrackingSection.tsx:205
#56 RepaymentSection.tsx:149
#57 RiskRatingJustificationSection.tsx:154
#58 ScorecardsSection.tsx:686
#59 ReviewPDFModal.tsx:142
#60 ReviewPDFModal.tsx:93
#61 ReviewPDFModal.tsx:91
(Hint: you previously found that several sections render HELP TIP HTML via
dangerouslySetInnerHTML in a modal, e.g. CustomerInfoSection ~:443,
CovenantsSection ~:950 – many of these flaws are likely those.)

Fix direction (Veracode-recognised cleanser):
- Call DOMPurify.sanitize(value ?? "", CONFIG) DIRECTLY inside each
  dangerouslySetInnerHTML / innerHTML / srcDoc expression. Do NOT hide the
  call inside a custom wrapper function (Veracode may not recognise it). A
  single shared CONFIG constant (options object) in one file is fine.
- CONFIG must keep everything our content legitimately uses, so normal
  content renders identically:
  * Rich text editor output: p, div, span, br, b/strong, i/em, u, h1, h2,
    ul/ol/li, a (href), font (face/size/color), inline style (color,
    background-color, text-align, width, etc.).
  * Tables incl. Excel/Word paste: table, thead, tbody, tr, th, td,
    colgroup, col, with width/colspan/rowspan/style attributes.
  * Images: img with data:image/... src (pasted screenshots) and width/
    height/style.
  * Help tips (03_LIBRARY_06_Help Tips): font, strong, div, span with style.
  * Word-pasted markup: namespaced tags like <o:p> may be dropped but their
    CONTENT must be kept (no text or images lost).
  * Links: keep href for http/https/mailto/relative; strip javascript: and
    other dangerous schemes; add rel="noopener noreferrer" if target is
    used (do not change target behaviour otherwise).
  * Must remove: script, style, iframe/object/embed, on* event handlers,
    javascript:/vbscript: URLs, srcdoc.
- If a field is actually plain text (no HTML ever), render {value} instead
  of HTML – only if you can prove it is plain text.
- ReviewPDFModal: sanitize the HTML before it goes into the iframe/print/
  srcDoc content. The generated PDF (@react-pdf, HtmlRichText.tsx) must stay
  byte-for-byte identical – do NOT change HtmlRichText.tsx or PDF
  components.
- Do NOT touch RichTextEditor.tsx editing behaviour or any data saved to the
  DB. Sanitize on render only.

Next.js SSR / dependency rules (STOP and ask if unsure):
- "use client" components are still pre-rendered on the server in the App
  Router. Plain `dompurify` needs a DOM; on the server it may return the
  input unchanged or throw. Check how each sink renders (SSR vs
  client-only/after mount) and make sure there is no server crash, no
  hydration mismatch, and no unsanitized HTML in the server HTML.
- Check package.json/lockfile for dompurify / isomorphic-dompurify /
  @types/dompurify. If a NEW dependency is required, STOP and ask me with
  options (A: dompurify client-only render, B: isomorphic-dompurify, C:
  other) incl. bundle size, server impact, licence, and whether the bank's
  package approval may be needed.

Work in 4 phases:

PHASE 1: READ-ONLY DISCOVERY (no edits)
For EVERY flaw: show the code at file:line, the sink (dangerouslySetInnerHTML
/ innerHTML / srcDoc / document.write), the data source (API field name,
help tip, user rich text), whether it renders on the server or client-only,
and classify REAL or FALSE POSITIVE with a one-line reason. List any other
sink in the frontend with the same pattern that Veracode did NOT flag
(report only – fix only the 14 unless I say otherwise). Check package.json.
If a decision is needed, STOP and ask with A/B/C options.

PHASE 2: PLAN – smallest generic change, one CONFIG, same pattern at every
sink. No refactors, renames or formatting-only changes.

PHASE 3: IMPLEMENT – touch only the needed lines.

PHASE 4: VERIFY + REPORT – verify with REAL content, not only mock:
- the local file test-data/rrj-20120.html (real Risk Rating Justification
  with Excel table, Word table, MsoNormal <o:p>, data-URI images) and a
  real help tip HTML (font/strong/div styles);
- malicious samples: <img src=x onerror=alert(1)>, <script>alert(1)</script>,
  <a href="javascript:alert(1)">x</a>, <svg onload=alert(1)>,
  <iframe src=...>, style="background:url(javascript:...)".
Show sanitized output: real content unchanged (or only harmless attribute
differences listed), malicious parts removed.

Report:
1. Table: Flaw ID | file:line (current) | sink | data source | REAL/FP |
   change made OR "Mitigate by Design" text ready to paste into Veracode
2. Root cause
3. ADDED lines (file:line)
4. REMOVED / CHANGED lines (file:line)
5. On-screen behaviour before vs after for each section, help tip modals,
   and the PDF modal / generated PDF
6. NULL / undefined / empty string: no crash, no "null"/"undefined" text,
   empty shows blank
7. Other screens unaffected + other unflagged sinks found (not changed)
8. npm run build + lint + type-check results (lint may be broken repo-wide –
   report separately), SSR/hydration check result
9. git status (no harness/temp files; test-data/ not staged)
Do not commit or push.
