Edit files. Auto-approve OFF. Output diffs for review, NO auto-apply.
Do NOT change any parsing/decoding logic. Do NOT touch other PDF components.

Goal: Fix "≥"→"e", "≤"→"d", en dash "–", and viewer-dependent "&" in the
Initial Memo, Final Memo, and CAS Linesheet PDFs. Root cause confirmed: these
use fontFamily "Helvetica" (react-pdf built-in), which lacks those glyphs. Fix
by registering an embedded Unicode font (DejaVu Sans) centrally and pointing
ONLY these three components to it.

STEP 1 — Central registration in frontend/src/components/pdf/pageSetup.ts
(the file that already has ensureHyphenationDisabled / registerHyphenationCallback):
  - Add an exported function ensureFontsRegistered() guarded by a boolean flag
    (same pattern as hyphenationRegistered) so it runs once.
  - Inside, call Font.register for family "DejaVuSans" with regular + bold from
    these CDN URLs:
      regular: https://cdn.jsdelivr.net/npm/dejavu-fonts-ttf@2.37.3/ttf/DejaVuSans.ttf
      bold:    https://cdn.jsdelivr.net/npm/dejavu-fonts-ttf@2.37.3/ttf/DejaVuSans-Bold.ttf
    Register regular as fontWeight "normal" and bold as fontWeight "bold" under
    the same family "DejaVuSans".
  - Wrap in try/catch with a no-op fallback, matching the existing hyphenation code.
  - If this codebase cannot load fonts from a URL at runtime and requires a local
    .ttf file instead, STOP and tell me — do not guess.

STEP 2 — In InitialMemoPDF.tsx and FinalMemoPDF.tsx:
  - Import and call ensureFontsRegistered() wherever ensureHyphenationDisabled()
    is already called (or at module top the same way).
  - Replace fontFamily: "Helvetica" with fontFamily: "DejaVuSans" in THIS file
    only. Keep bold via fontWeight so headings/strong stay bold.

STEP 3 — CAS Linesheet component:
  - First tell me the exact file path that renders the CAS Linesheet (identify by
    the "CAS Linesheet" footer text). Do NOT edit it until you have named it.
  - Then apply the same two changes: call ensureFontsRegistered() and replace
    fontFamily: "Helvetica" with "DejaVuSans" in that file only.

Also: HtmlRichText.tsx inherits the parent Page font, so if the memo Page's
fontFamily is set to "DejaVuSans", the rich-text narrative (where ≥/≤ appear)
will inherit it. Confirm whether HtmlRichText sets its own fontFamily anywhere;
if it hardcodes "Helvetica" internally, change that to "DejaVuSans" too (this is
the component that actually renders the ≥/≤ text). Report what you find.

Show all diffs. No auto-apply.
