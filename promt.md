Edit ONLY this file: frontend/src/components/pdf/HtmlRichText.tsx
Do not touch any other file. Make a manual diff review possible — keep changes minimal and localized to the two functions named below. Auto-approve stays OFF.

We are fixing two PDF-rendering bugs in the Rich Text → react-pdf pipeline:

BUG #192 — Encoded HTML tags render as literal text.
Root cause: parseHtmlToAst() scans the raw input for "<" to tokenize tags, but
content pasted from Word arrives entity-encoded (e.g. "&lt;div&gt;",
"&lt;span style=&quot;font-size: 0.875rem&quot;&gt;"). Because these are never
decoded BEFORE tokenization, they are treated as text and appear verbatim in
the PDF. The existing comment already says input should be pre-decoded, but the
code does not do it.

BUG #191 — Named HTML entities are not decoded.
Root cause: decodeEntities() handles &nbsp; &lt; &gt; &quot; &#39; &apos;
numeric (&#dec; / &#xhex;) and &amp;, but omits common NAMED entities. In
particular &ge; and &le; (≥ and ≤) survive as literal text, and other pasted
symbols like &ndash; &mdash; &hellip; &rsquo; etc. also survive.

Make exactly these two changes:

CHANGE 1 — Extend decodeEntities() with a named-entity map.
Add the following named entities to the replacement chain, applied BEFORE the
"&amp; MUST be last" line (so ampersand decoding still runs last):

  &ge; → "≥"        &le; → "≤"
  &ndash; → "–"     &mdash; → "—"
  &hellip; → "…"
  &lsquo; → "‘"     &rsquo; → "’"
  &ldquo; → "“"     &rdquo; → "”"
  &bull; → "•"      &middot; → "·"
  &times; → "×"     &divide; → "÷"
  &deg; → "°"       &plusmn; → "±"
  &copy; → "©"      &reg; → "®"      &trade; → "™"
  &euro; → "€"      &pound; → "£"    &yen; → "¥"     &cent; → "¢"

Keep it as case-insensitive .replace() calls consistent with the existing
style, or a single lookup map — your choice, but keep the multi-pass loop and
keep &amp; strictly last. Do NOT remove or reorder any existing replacement.

CHANGE 2 — Pre-decode encoded markup before tokenization in parseHtmlToAst().
At the very start of parseHtmlToAst(), before any scanning for "<", detect
whether the input contains encoded tags (i.e. it contains "&lt;" or "&gt;" and
does NOT already contain a real "<" ... ">" tag). If it looks entity-encoded,
run a single decode pass so "&lt;div&gt;" becomes "<div>" and can be tokenized
as real markup.

Guard against double-decoding of legitimately-escaped text: only pre-decode the
angle-bracket/quote entities needed for tag recognition here
(&lt; &gt; &quot; &#39; &apos; and numeric forms) — do NOT decode &amp; in this
pre-pass, so that "&amp;lt;" intended as literal "&lt;" is preserved. Text-run
decoding via decodeEntities() still runs afterward as today.

Constraints:
- Do not change styles, table rendering, or image rendering (those are separate
  tickets pending approval).
- Do not modify InitialMemoPDF.tsx / FinalMemoPDF.tsx.
- Preserve the existing 5-pass loop and the "&amp; last" invariant.
- Output only the edited HtmlRichText.tsx changes for diff review; do not
  auto-apply.
