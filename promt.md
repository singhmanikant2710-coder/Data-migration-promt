Bug 222 fix — CORE Export (02_CORE_02_Reviews CSV): strip raw HTML from rich-text columns, add UTF-8 BOM, remove NOLOCK + add ORDER BY. Cancelled reviews MUST stay included (Geoff confirmed). SINGLE FILE. No new packages — use built-in Regex + System.Net.WebUtility only. Show diff before applying.

FILE: backend/src/Casrr.Api/Controllers/ExportsController.cs

1. HTML strip: columns Borrower_information, Transaction_information, Covenant_information, Policy_exception_information, Regulatory_flag_information, Collateral_information, PSOR_information, SSOR_information, Key_risks, Scorecard_information, Risk_rating_justification contain raw HTML (RichTextEditor innerHTML) incl. base64 <img> data URIs. Add a private StripHtml(string) helper that: removes <img ...> tags first (replace with empty), removes all remaining tags via Regex (<[^>]+>), decodes entities via System.Net.WebUtility.HtmlDecode, collapses whitespace/newlines into a single space, trims. Apply StripHtml to every string cell value before EscapeCsv. Do NOT apply to non-string types.

2. Add UTF-8 BOM: change writer encoding to new UTF8Encoding(encoderShouldEmitUTF8Identifier: true) so Excel reads UTF-8 correctly (fixes â€" smart quotes/dashes).

3. Remove WITH (NOLOCK) from the SELECT and add ORDER BY Review_id for deterministic order (eliminates dirty-read duplicate/skip artifacts). Keep SELECT *.

4. Do NOT add any Cancelled filter — cancelled reviews must remain in the export (Geoff confirmed). Do NOT change the route, auth, or download flow. No new package.

List every line changed. Commit: "Fix Bug 222: strip HTML from rich-text columns, add UTF-8 BOM, remove NOLOCK + ORDER BY in CORE Reviews export (cancelled reviews retained)".
