ROLE
You are a senior presentation engineer. Update the EXISTING PowerPoint deck in place using python-pptx.

STRICT CONSTRAINTS (MANDATORY)
- Edit ONLY this file in place: FHN_Insurance_NET_Complete_Scope_16_Week_Development_Plan.pptx
- Do NOT create a new .pptx, a copy, or any additional output file.
- Make ONLY the changes listed below. Do not change any other text, number, slide, layout, colour, font, footer, slide order or appendix slide.
- Preserve the existing theme (dark navy + teal), fonts and styling.
- If any instruction is ambiguous or cannot be done exactly, STOP and ask before changing anything.
- Do not commit or push.

==================================================
PHASE 1 – READ-ONLY DISCOVERY (no changes)
==================================================
1. List all shapes on Slide 1 and Slide 3 with shape name, position, size, fill colour, border colour and text.
2. Identify on Slide 1 the 4 KPI cards (45 Screens/Forms, 29 Reports + Exports, 13 Business Modules, 16 weeks Development Timeline) and their current fill/accent colours.
3. Identify on Slide 3 the 8 numbered cards, their current text (keep the current edited text exactly as it is in the file), and the 3 "ACTION REQUIRED" tags.

==================================================
PHASE 2 – CHANGES
==================================================

SLIDE 1 – KPI row
1. Add ONE new KPI card in the same style as the existing cards:
   - Value: "20+"
   - Label: "SCREENS OUT OF SCOPE"
   - Small sub-label (smaller font, italic): "Pending business confirmation"
2. New KPI order (left to right): 45 Screens / Forms | 29 Reports + Exports | 13 Business Modules | 20+ Screens Out of Scope | 16 weeks Development Timeline.
3. Resize all 5 cards to equal width with equal spacing, fitting within the same horizontal area the current 4 cards occupy. Keep the same card height and vertical position. No text wrapping into a second line for values.
4. Colour rule:
   - The 4 scope cards (Screens, Reports, Business Modules, Screens Out of Scope) must share the SAME style: current light grey fill with the existing teal top accent bar, navy value text.
   - The "16 weeks / DEVELOPMENT TIMELINE" card must be visually distinct: solid teal fill (same teal as the theme accent), white value and label text, no grey fill.
5. Do not change the complexity table, the "Also in Scope" panel, the roadmap boxes or any other Slide 1 element, except shifting nothing other than the KPI cards.

SLIDE 3 – Assumptions and Dependencies
1. Title text: change "Assumptions, Dependencies" to "Assumptions and Dependencies". Keep subtitle "KEY CONSIDERATIONS – FYI".
2. Regroup the existing 8 cards into TWO clearly separated container boxes, side by side (left and right), each with a header bar in the theme style:

   LEFT BOX – header: "DEPENDENCIES (Client Action Required)"
   - 1. Business SME engagement from Day 1
   - 2. Scope confirmation – 20+ additional screens
   - 8. QA strategy required
   Keep the "ACTION REQUIRED" tag on each of these three items.

   RIGHT BOX – header: "ASSUMPTIONS"
   - 3. Actual timelines will be published accounting for holidays and PTOs
   - 4. Development scope only
   - 5. Production support priority will need adjustment of timelines
   - 6. Single-developer estimate
   - 7. Progressive complexity discovery

3. Keep each item's existing heading and description text exactly as currently in the file. The ONLY text edit allowed is in item 3's heading: "accounting the holidays" → "accounting for holidays".
4. Keep the original numbers (1–8) and teal number badges so references stay consistent.
5. Layout: both boxes equal height, aligned to the same top and bottom, consistent padding, minimum font 10 pt, no text overflow, no overlapping shapes, nothing extending beyond slide margins or over the footer.
6. Remove the old individual card borders/placement only as needed to fit items inside the two container boxes.

==================================================
PHASE 3 – VERIFY AND REPORT
==================================================
1. Confirm the file was saved in place and no new .pptx file was created.
2. Confirm Slides 2, 4, 5, 6, 7 are unchanged (compare text and shape count before vs after).
3. Render Slide 1 and Slide 3 to PNG and visually check: alignment, equal card widths, distinct colour of the 16-weeks card, no overflow, no overlap.
4. Report:
   - ADDED: (slide, shape name, text)
   - REMOVED: (slide, shape name, text)
   - MODIFIED: (slide, shape name, before → after)
   - Any instruction you could not complete exa
