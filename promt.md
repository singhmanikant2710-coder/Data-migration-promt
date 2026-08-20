Edit ONLY frontend/src/components/pdf/pageSetup.ts. Auto-approve OFF. Output diff, no auto-apply.

The font files are now served from the same public/assets folder as the existing
logos, at:
  frontend/public/assets/fonts/DejaVuSans.ttf
  frontend/public/assets/fonts/DejaVuSans-Bold.ttf

In ensureFontsRegistered(), replace the two jsDelivr CDN `src` URLs with these
public URL paths (note: the "public" segment is NOT part of the URL):
  regular -> src: "/assets/fonts/DejaVuSans.ttf",  fontWeight: "normal"
  bold    -> src: "/assets/fonts/DejaVuSans-Bold.ttf", fontWeight: "bold"

Confirm this matches how existing PDF components reference logos from
public/assets (e.g. FHB_Logo.png is referenced as "/assets/FHB_Logo.png"). If
the logo pattern differs, use that same proven pattern for the fonts.

Keep family "DejaVuSans", the guard flag, and try/catch. Do NOT add any webpack
rule, next.config change, or TS declaration — this needs none. Change nothing
else. Show the diff.
