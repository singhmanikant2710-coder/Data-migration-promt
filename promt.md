Edit ONLY these 2 files. Auto-approve OFF. Output diffs, no auto-apply.
  1. frontend/src/components/pdf/ReviewPDF.tsx
  2. frontend/src/components/pdf/ReviewPDFModal.tsx

Context: The memo the user downloads is generated in ReviewPDFModal.tsx, which
renders <ReviewPDF> to a blob. ReviewPDF's Page still uses fontFamily "Helvetica"
(confirmed), which lacks ≥/≤ glyphs → renders as e/d. Also ReviewPDFModal never
registers fonts and never waits for them before pdf().toBlob(). react-pdf 4.3.2
has no Font.load(), so we prefetch the .ttf URLs into browser cache first.

CHANGE 1 — ReviewPDF.tsx:
In the Page/root style, change fontFamily: "Helvetica" to fontFamily: "DejaVuSans".
Change ONLY the body/Page font. Keep bold working via fontWeight. Do not alter
layout/spacing values.

CHANGE 2 — ReviewPDFModal.tsx:
Import ensureFontsRegistered from the pageSetup module (match the existing import
path style used in this file/folder). Inside the async run() in useEffect,
IMMEDIATELY BEFORE the line that does `await pdf(<ReviewPDF ... />).toBlob()`,
insert:

  ensureFontsRegistered();
  const origin = window.location.origin;
  await Promise.all([
    fetch(`${origin}/assets/fonts/DejaVuSans.ttf`,      { cache: "reload" }).then(r => r.arrayBuffer()),
    fetch(`${origin}/assets/fonts/DejaVuSans-Bold.ttf`, { cache: "reload" }).then(r => r.arrayBuffer()),
  ]);

Wrap the prefetch in a try/catch so a fetch failure does not block PDF generation
(on failure, log and continue). Do not change anything else in either file.
Show both diffs.
