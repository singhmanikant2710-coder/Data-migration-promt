Bug 191 fix prep — READ-ONLY, no edits. One pass, answer, STOP. We will register+embed a Unicode TTF (DejaVu Sans) so ≥/≤ render correctly. Confirm the setup before we add it.

1. Is there any existing Font.register() call anywhere in the frontend PDF code? If yes, paste it (file/line) — the family name and src pattern (local path vs URL). If none, confirm there is zero font registration currently.
2. Where do the PDF components define their Page/base styles (the top-level <Page> or <Document> style with fontSize etc.)? List each PDF component file and the base style object where a fontFamily should be added. (InitialMemoPDF, FinalMemoPDF, ReviewPDF, CrmSummaryPDF, CrmFindingsObservationsPDF, ScorecardResultsPDF, and any shared pageSetup.ts.)
3. Is there a shared setup module (pageSetup.ts) imported by all PDF components where a single Font.register() + a shared fontFamily token could live, so we register once and apply consistently? 
4. Where are static assets served from in this Next.js app (public/ folder path)? Confirm the correct location + URL path to place a local DejaVuSans.ttf so @react-pdf can load it via src.
5. Does @react-pdf here load fonts from a URL, a local import (import font from '...ttf'), or a public path? Which pattern does this project's build support?

Report file paths + the base style objects + the correct asset location. Do NOT register or add anything yet.
