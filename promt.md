READ-ONLY first. Do NOT edit. Investigate and report.

Font registration for DejaVuSans is still not taking effect — PDFs render ≥/≤
as e/d even though the .ttf is served correctly and now uses an absolute URL.
Likely cause: @react-pdf/renderer loads registered fonts ASYNCHRONOUSLY, and the
PDF (pdf(doc).toBlob()) is generated before the font finishes downloading, so it
falls back to Helvetica.

Report only, no edits:

1. In downloadMemoPdf.tsx (and wherever the memo preview generates the PDF), show
   the exact sequence: when is ensureFontsRegistered() called relative to
   pdf(doc).toBlob()? Is there any await/wait for fonts before rendering?

2. Does @react-pdf/renderer in this version expose Font.load() or a way to await
   font readiness? Check the installed @react-pdf/renderer version in
   package.json and confirm what font-loading API it supports (Font.load,
   or awaiting registration).

3. Confirm whether ensureFontsRegistered() currently runs at MODULE TOP (import
   time) or inside the render function. If module-top, the font fetch may not be
   complete by first render.

4. Recommend the MINIMAL fix to guarantee the font is fully loaded before
   pdf(...).toBlob() runs — e.g. await Font.load({ fontFamily: "DejaVuSans" })
   for both weights inside the download/generate function, before generating the
   blob. Show exactly where to add the await and in which file.

Output: the current call sequence + the react-pdf version's font API + the exact
minimal await-based fix location. No edits.
