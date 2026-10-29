TASK: Create ONE single-slide, leadership-level PowerPoint titled "CASRR Enhancement" for the CASRR client Director (non-technical). Use the enhancement details I pasted above as the ONLY source of truth for the enhancements. Do NOT change any application code.

AUDIENCE & GOAL
The Director often says "AI will do it fast, why does it take so long?". The slide must answer that calmly and factually in 30 seconds:
- what we are delivering and how much (by priority),
- which screens/tabs are affected,
- dependencies and impact,
- why delivery (merge to QA) is 20 Nov 2026.
Tone: confident, plain business English, no technical jargon, no blame, no hype.

SCHEDULE FACTS (show clearly on the slide – do not change)
- Effort: 2 working weeks = 10 working days (development + full end-to-end regression).
- Team location: Gurugram (Gurgaon), India.
- Work Block 1: 29 Oct – 06 Nov 2026 (7 working days)
- 09 Nov 2026: Diwali public holiday (India)
- 10 Nov – 16 Nov 2026: planned leave (approved in advance) – no development
- Work Block 2: 17 Nov – 19 Nov 2026 (3 working days)
- 20 Nov 2026: Merge to QA (buffer/release day)
- Without the holiday and leave, the 2 weeks would have ended on 11 Nov.

STEP 1 – ANALYSIS (show in chat before building)
From the pasted enhancements, produce a table:
# | Enhancement (short plain-English name) | Priority (High / Moderate / Low – exactly as I gave) | Screen/Tab affected | Depends on / Impacts (other screens, reports, PDFs, emails, database)
- Count per priority and the total. Counts MUST add up to the total.
- Group by screen/tab (e.g. Review Form, Review Queue, Review Status, Review History, Reports, Admin/Maintenance, PDFs, Email).
- If any item has no priority or an unclear screen, LIST it and ask me – do not guess.
- Do not invent numbers, percentages, hours or dates that are not in my input or this prompt.

STEP 2 – BUILD THE SLIDE
Tool: python-pptx (pip install if needed). 16:9 (13.333 x 7.5 in). ONE slide only.
Save as: deliverables/CASRR_Enhancement_Plan.pptx (create the folder; it must NOT be committed).

Layout (top to bottom):
1. TITLE BAR: "CASRR Enhancement" (large, bold). Subtitle: "Delivery plan – merge to QA on 20 Nov 2026".

2. PRIORITY ROW – 4 boxes side by side:
   - High | Moderate | Low – each shows the COUNT large (about 44–48pt) with the label below.
   - All three priority boxes in ONE single colour: navy #1F3864, white text. Do NOT colour-code by severity.
   - TOTAL box: same navy family (lighter shade or outlined), with the total count.

3. MIDDLE – two columns:
   - Left, "Screens / Tabs impacted": Screen name – number of enhancements (from Step 1). Max ~7 lines; merge small ones into "Other".
   - Right, "Dependencies & impact": 3–4 short plain-English bullets from Step 1 (e.g. screens/reports touched by several changes, items needing DBA database scripts and approval, items waiting on business rule confirmation, Record Retention – permanent deletes – as highest risk if present).

4. TIMELINE STRIP (left to right), colours DIFFERENT from the navy priority boxes:
   - Work Block 1 (teal #0F766E, white text): "29 Oct – 06 Nov | High priority + start Moderate"
   - Holiday/Leave (light grey #D9D9D9, dark text): "09 Nov Diwali (India holiday) + 10–16 Nov planned leave – no development"
   - Work Block 2 (teal #0F766E, white text): "17 – 19 Nov | Remaining Moderate + Low + full end-to-end regression"
   - Milestone marker (flag/diamond) at the end: "20 Nov – Merge to QA"
   - Caption under the strip: "2 working weeks (10 days: 29 Oct – 19 Nov), split around the Diwali holiday and planned leave; 20 Nov reserved for QA merge."
   Adjust which priorities sit in each block to what Step 1 shows; keep it realistic (regression must be in Block 2).

5. BOTTOM PANEL – "Why 20 Nov (even with AI)" – 5 short one-line bullets:
   - AI speeds up writing code, but most time goes into understanding existing business rules and making sure nothing else changes.
   - Each change touches shared screens, reports and PDFs – every one is checked for side effects.
   - No dedicated QA team: we run a full end-to-end regression ourselves so existing functionality does not break.
   - Some items need business confirmation and DBA database scripts/approvals before release.
   - Diwali holiday (9 Nov) and planned leave (10–16 Nov) in India – the 10 working days are split around them.

6. FOOTER (small, grey): "CASRR – Credit Assurance Services RiskReview | First Horizon"

DESIGN RULES
- Clean, generous white space. Colours only: navy (priority boxes), teal (work blocks), light grey (holiday/leave), dark/grey text.
- Font: Calibri or Segoe UI. Body min 12pt; counts large.
- No logos, images, clip-art or animations.
- Everything must fit on ONE slide with no text overflowing or overlapping. If it does not fit, shorten wording – do not go below 12pt.
- Speaker notes (2–3 sentences) the presenter can say to the Director, including that 20 Nov already accounts for the Diwali holiday and planned leave.

STEP 3 – VERIFY + REPORT (in chat)
- Convert to PNG (LibreOffice: soffice --headless --convert-to png) and visually check for overflow/overlap; fix and re-check.
- Report: file path, the Step 1 table, counts per priority + total (confirm they sum correctly), assumptions made, and any items you need me to confirm.
- git status must show no changes to application code. Do not commit or push.
