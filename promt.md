READ-ONLY. Do NOT edit. Investigate font sizes only.

The memo/linesheet PDFs now use DejaVuSans (wider than the old Helvetica),
causing text overlap in tight areas: header ("Completed Date" value touching
"Reviewer"), Account Information table headers ("Bank" clipped), and long account
numbers. We want to reduce the base body font size slightly so DejaVuSans fits
like Helvetica did.

Report only, no edits:

1. In pageSetup.ts, show the fontSizes object (all keys and their point values,
   e.g. value, label, small, etc.). Which key is used for body/table text?

2. In InitialMemoPDF.tsx, FinalMemoPDF.tsx, and ReviewPDF.tsx: what fontSize is
   applied to (a) the header block (Customer #, Completed Date, Reviewer, etc.),
   (b) the Account Information table cells, (c) general body text? List the exact
   current values and whether they come from pageSetup.fontSizes or inline
   numbers.

3. If I want to reduce the relevant body/table/header font size to 9pt, tell me
   EXACTLY which values/keys to change in which files, and whether changing a
   shared pageSetup.fontSizes key would also affect OTHER PDFs (the 15+ other
   components) — I only want to affect these memo/linesheet PDFs, not everything.

4. Confirm 9pt stays readable for a banking report and flag anything that might
   look too small (e.g. footnotes already at 8pt).

Output: fontSizes object + per-file current sizes + exact change plan scoped to
just these 3 files + shared-vs-local warning. No edits.
