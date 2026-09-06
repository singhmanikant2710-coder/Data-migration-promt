Two follow-ups on Bug 192, same session. Do A (read-only) first, report, then apply B and C.

A) COMPLETENESS SCAN — read-only, no edits. Scan EVERY PDF component in frontend/src/components/pdf/ yourself. Find every site that renders a DB rich-text field (any field a user fills via RichTextEditor and saved as innerHTML — comments, information, description, justification, key_risks, mitigation, rationale, guidance, notes, etc.). For each such site, classify it as: routes via HtmlRichText, OR applies stripHtml/stripHtmlToText, OR passes RAW via out()/plain <Text> (a LEAK). Report every LEAK site you find (file + line + field), or confirm there are none beyond what we already fixed.

B) In ReviewPDF.tsx the findings DESCRIPTION field is still rendered raw while comments are now cleaned — apply the same stripHtml to it for consistency.

C) For any additional leak found in A, fix it using the SAME pattern already applied in this session (route through HtmlRichText with an outHtml/stripHtmlToText fallback, or apply stripHtml for plain cells). Keep the /[<>&]/ gate so non-HTML values render byte-identical to before. Do NOT touch HtmlRichText's parser logic.

Report the full scan results, then every file + line changed in B and C, with diffs. Do NOT commit — leave staged for my review.
