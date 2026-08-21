Apply the SAME working DejaVuSans font fix (that already works for CAS Linesheet)
to the Initial Memo and Final Memo PDFs. Auto-approve OFF. Output diffs, no
auto-apply. Edit ONLY these 4 files.

Confirmed: memos are generated client-side. downloadMemoPdf.tsx and
MemoPdfModal.tsx call pdf(doc).toBlob() (and MemoPdfModal also previews via
PDFViewer) but neither calls ensureFontsRegistered() or prefetches the .ttf.
InitialMemoPDF.tsx and FinalMemoPDF.tsx still use fontFamily "Helvetica".
Reference working pattern: ReviewPDFModal.tsx.

Changes:

FILE 1 — frontend/src/components/pdf/InitialMemoPDF.tsx
  Change Page-level fontFamily from "Helvetica" to "DejaVuSans". Keep bold via
  fontWeight. Nothing else.

FILE 2 — frontend/src/components/pdf/FinalMemoPDF.tsx
  Same: Page fontFamily "Helvetica" -> "DejaVuSans".

FILE 3 — frontend/src/app/review/[ecif]/review-info/components/downloadMemoPdf.tsx
  Import ensureFontsRegistered from the pageSetup module (match existing import
  style). Immediately BEFORE `const blob = await pdf(doc).toBlob();` add:
      ensureFontsRegistered();
      try {
        const origin = window.location.origin;
        await Promise.all([
          fetch(`${origin}/assets/fonts/DejaVuSans.ttf`, { cache: "reload" }).then(r => r.arrayBuffer()),
          fetch(`${origin}/assets/fonts/DejaVuSans-Bold.ttf`, { cache: "reload" }).then(r => r.arrayBuffer()),
        ]);
      } catch (e) { try { console.warn("Font prefetch failed; continuing.", e); } catch {} }

FILE 4 — frontend/src/app/review/[ecif]/review-info/components/MemoPdfModal.tsx
  a) In the onDownload handler, add the SAME ensureFontsRegistered() + prefetch
     block immediately before `const blob = await pdf(doc).toBlob();`.
  b) The preview uses <PDFViewer> which also needs the font. Ensure
     ensureFontsRegistered() runs before the PDFViewer renders, and warm the
     fonts once when the modal mounts: in a useEffect (on mount / when data is
     ready, before/at first render of PDFViewer), call ensureFontsRegistered()
     and do the same .ttf prefetch (guard so it runs once). This mirrors how the
     linesheet path warms fonts before generating. If the component has no
     useEffect yet, add a minimal one. Do not change unrelated rendering.

Keep everything else unchanged. Show all 4 diffs. No auto-apply.
