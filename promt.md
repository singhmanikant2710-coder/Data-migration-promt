Bug 207 follow-up — Review Form Covenants section. When "No Monitoring Covenants" or "No Performance Covenants" is selected as the Covenant Type, default the row's Frequency and Last Eval Status fields to "N/A". "N/A" must be added as a selectable option in both dropdowns if not already present, so it can be saved. READ-ONLY, no edits. One pass, answer, STOP.

Investigate:
1. Find the Covenants section in the Review Form (CovenantsSection.tsx or similar) — the table/form where users add covenant rows with Covenant Type, Frequency, Last Eval Status, Threshold, etc.
2. Find where "No Monitoring Covenants" / "No Performance Covenants" are selectable as Covenant Type values (this is the same feature/data used in Bug 207's PDF fix — the covenant library). File + line.
3. Find the Frequency field and the Last Eval Status field for a covenant row: are they free-text, or dropdowns/selects with a fixed option list? File + line + current option list.
4. Does "N/A" already exist as an option in either dropdown? If not, where would it need to be added (a static option list in the component, or a library-driven list from the backend/covenant library)?
5. Is there existing logic anywhere that reacts to Covenant Type changing (e.g. the segment→unit/market cascade we just fixed in CustomerInfoSection) that could be mirrored here — i.e. an onChange handler on the Covenant Type field that could set Frequency/Last Eval Status to "N/A" when the "No ... Covenants" type is selected?
6. Report exactly: (a) the two field locations, (b) whether "N/A" needs adding to their option lists, (c) the best hook point to set the default (onChange of Covenant Type, mirroring a pattern we've already used elsewhere), and (d) whether this needs a backend/DTO change or is purely a frontend default.

Report file paths + line numbers + current option lists. Do NOT propose or write a fix yet.
