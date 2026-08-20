Edit ONLY frontend/src/components/pdf/pageSetup.ts. Auto-approve OFF. Output diff, no auto-apply.

The font file IS served correctly (opening http://localhost:3000/assets/fonts/DejaVuSans.ttf
downloads it fine). But PDFs still render ≥/≤ as e/d, meaning @react-pdf/renderer
did not register DejaVuSans and fell back to Helvetica. Cause: react-pdf in the
browser cannot resolve the relative "/assets/fonts/..." path; it needs an
ABSOLUTE URL including origin.

Change ensureFontsRegistered() to build absolute URLs from the runtime origin,
client-side only:

  const origin = typeof window !== "undefined" ? window.location.origin : "";
  Font.register({
    family: "DejaVuSans",
    fonts: [
      { src: `${origin}/assets/fonts/DejaVuSans.ttf`,      fontWeight: "normal" },
      { src: `${origin}/assets/fonts/DejaVuSans-Bold.ttf`, fontWeight: "bold"   },
    ],
  });

Keep the fontsRegistered guard flag and the try/catch no-op. Do not change
anything else. Show the diff.
