Bug 192 — PDF renders raw HTML jargon (<div>, <span style="font-size:...">, &nbsp;) from Word-pasted rich text. READ-ONLY, no edits. One pass, answer, STOP.

Screenshot shows PDF Comments cells containing literal "<div>", "<span style="font-size: 0.875rem;">", "&nbsp;" instead of clean text. This is Word-pasted content saved as innerHTML by RichTextEditor.

Investigate HtmlRichText.tsx (the PDF rich-text parser):
1. How does it parse HTML? Does it handle <span> and <div> tags? Does it strip/ignore inline style attributes (style="font-size:...")? Or does it emit unknown/styled tags as literal text (which would explain the visible <span ...> jargon)?
2. Does it decode HTML entities like &nbsp; (U+00A0), &amp;, &lt;, &gt;, &#39;? Where (line)? Is &nbsp; being left literal in some path?
3. Trace: for an input like '<div>Text <span style="font-size: 0.875rem;">more</span>&nbsp;end</div>', which render path handles it, and where would the literal <span ...> and &nbsp; leak through as visible text? Identify the exact gap (unknown-tag handling, style-attribute handling, entity decoding).
4. Is there an existing stripHtmlToText / decodeEntities helper in this file or nearby that already does this correctly, which the leaking path should be using but isn't?
5. Separately: on the EDITOR side (RichTextEditor.tsx), when content is pasted, is there any paste sanitization (onPaste handler stripping styles/tags)? Or does it accept raw pasted HTML verbatim?

Report file paths + line numbers + the exact gap where HTML/entities leak into the PDF. Do NOT fix yet.
