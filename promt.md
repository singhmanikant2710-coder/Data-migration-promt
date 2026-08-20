Edit ONLY frontend/src/components/pdf/pageSetup.ts. Auto-approve OFF. Output diff, no auto-apply.

Switch ensureFontsRegistered() from jsDelivr CDN URLs to LOCAL bundled font files.
CDN fetch is unreliable in this environment; local files are required.

Font files are now at:
  frontend/src/assets/fonts/DejaVuSans.ttf
  frontend/src/assets/fonts/DejaVuSans-Bold.ttf
First verify this exact folder and both files exist. If elsewhere, stop and tell me.

Replace the two CDN `src` URLs in Font.register with the local files, using the
pattern that actually works in this Next.js + @react-pdf/renderer project. Prefer:
  import DejaVuSans from "../assets/fonts/DejaVuSans.ttf";
  import DejaVuSansBold from "../assets/fonts/DejaVuSans-Bold.ttf";
  ...src: DejaVuSans ... src: DejaVuSansBold
(adjust the relative path from pageSetup.ts). 

If .ttf module imports require a webpack/Next.js asset-loader or next.config
change to work, DO NOT add it silently — report exactly what config is needed
and where, and STOP for my approval. As an alternative, tell me if moving the
fonts to frontend/public/fonts and using src: "/fonts/DejaVuSans.ttf" would be
simpler and needs no config.

Keep family "DejaVuSans", normal + bold, the guard flag, try/catch. Change
nothing else. Show the diff.
