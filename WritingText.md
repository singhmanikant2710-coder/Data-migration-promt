TASK: Create ONE single-slide, leadership-level PowerPoint for the CASRR client Director (non-technical). Use the enhancement details I pasted above as the ONLY source of truth. Do NOT change any application code.

AUDIENCE & GOAL
The Director often says "AI will do it fast, why does it take so long?". The slide must answer that calmly and factually in 30 seconds: what we are delivering, how much, which screens, what it impacts, and why the delivery date is 20 Nov 2026. Tone: confident, plain business English, no technical jargon (no "API", "repository", "schema", "regression suite" without a plain explanation), no blame, no hype.

SCHEDULE FACTS (must be shown clearly on the slide)
- Effort: 2 weeks (10 working days) of development + end-to-end regression.
- Team location: Gurugram (Gurgaon), India.
- 9 Nov 2026: Diwali public holiday (India).
- 10 Nov – 16 Nov 2026: planned leave (approved in advance).
- Therefore the 2 working weeks are split around the holiday/leave, and delivery (merge to QA) is 20 Nov 2026.
- Work Block 1: [START DATE] – 06 Nov 2026
- Holiday + planned leave: 09 Nov – 16 Nov 2026 (no development)
- Work Block 2: 17 Nov – 20 Nov 2026 (remaining items + full end-to-end regression)
- Milestone: Merge to QA – 20 Nov 2026

STEP 1 – ANALYSIS (show in chat before building)
From the pasted enhancements, produce a table: # | Enhancement (short plain-English name) | Priority (High/Moderate/Low – exactly as I gave) | Screen/Tab affected | Depends on / Impacts (other screens, reports, PDFs, emails, database).
- Count per priority and the total. The counts MUST add up to the total.
- Group by screen/tab (e.g. Review Form, Review Queue, Review Status, Reports, Admin/Maintenance, PDFs, Email).
- If any item has no priority or unclear screen, LIST it and ask me – do not guess.
- Do not invent numbers, percentages, hours or dates that are not in my input or this prompt.

STEP 2 – BUILD THE SLIDE
Tool: python-pptx (pip install if needed). 16:9 (13.333 x 7.5 in). One slide only. Save as: deliverables/CASRR_Enhancement_Plan.pptx (create folder; this folder must NOT be committed).

Layout (top to bottom):
1. Title bar: "CASRR Enhancement" (large, bold). Small subtitle: "Delivery plan – merge to QA by 20 Nov 2026".
2. Row of 4 boxes:
   - 3 PRIORITY boxes: High | Moderate | Low – each shows the COUNT big (e.g. 48pt) and the label below. All three boxes in ONE single colour (navy #1F3864, white text). Do NOT colour-code by severity.
   - 1 TOTAL box (same navy, slightly lighter outline) with total count.
3. Middle-left: "Screens / Tabs impacted" – compact list: Screen name – number of enhancements (from Step 1). Max ~7 lines; merge small ones into "Other".
4. Middle-right: "Dependencies & impact" – 3 to 4 short bullets in plain English, from Step 1 (e.g. shared screens/reports affected by multiple changes, items needing DBA database scripts, items needing business rule confirmation, permanent-delete Record Retention as highest risk if present).
5. TIMELINE strip (horizontal, left to right), in colours DIFFERENT from the navy priority boxes:
   - Work Block 1 (teal #0F766E, white text): "[START DATE] – 06 Nov | High priority + start Moderate"
   - Holiday/Leave block (light grey #D9D9D9, dark text, clearly labelled): "09 Nov Diwali (India holiday) + 10–16 Nov planned leave – no development"
   - Work Block 2 (teal #0F766E, white text): "17 – 20 Nov | Remaining Moderate + Low + full end-to-end regression"
   - Milestone marker at the end (diamond/flag): "Merge to QA – 20 Nov 2026"
   - Small caption under the strip: "2 working weeks (10 days), split around the Diwali holiday and planned leave."
   Adjust which priorities go in each block to what Step 1 actually shows; keep it realistic (regression must be in Block 2).
6. Bottom panel: "Why 20 Nov (even with AI)" – 5 short bullets, plain English:
   - AI speeds up writing code, but most of the time goes into understanding the existing business rules and making sure nothing else changes.
   - Each change touches shared screens, reports and PDFs – every one is checked for side effects.
   - No dedicated QA team: we run a full end-to-end regression ourselves after all changes, so existing functionality does not break.
   - Some items need business confirmation and DBA database scripts/approvals before release.
   - Diwali holiday (9 Nov) and planned leave (10–16 Nov) in India – the 2 working weeks are split around them.
   (Keep each bullet to one line if possible.)
7. Footer (small, grey): "CASRR – Credit Assurance Services RiskReview | First Horizon".

Design rules:
- Clean, lots of white space. Colours: navy (priority boxes) + teal (work blocks) + light grey (holiday/leave) + grey text only.
- Font: Calibri/Segoe UI. Min font size 12pt (body), counts large.
- No logos, no images, no clip-art, no animations.
- Everything must fit on ONE slide with no text overflowing boxes. If it does not fit, shorten wording – do not shrink below 12pt.
- Add speaker notes (2–3 sentences) the presenter can say to the Director, including that the 20 Nov date already accounts for the Diwali holiday and planned leave.

STEP 3 – VERIFY + REPORT (in chat)
- Convert to PNG (LibreOffice: soffice --headless --convert-to png) and visually check for overflow/overlap; fix and re-check.
- Report: file path, the Step 1 table, counts per priority + total (and that they sum correctly), any assumptions, any items you need me to confirm.
- git status must show no changes to application code. Do not commit or push.
