ROLE
You are a senior .NET modernization consultant and presentation engineer. Your task is to update an existing client-facing PowerPoint deck using python-pptx, and to source screen and report names directly from the legacy application already available in this workspace.

INPUTS
1. Existing deck: FHN_Insurance_NET_Complete_Scope_4_Month_Development_Plan.pptx (3 slides).
2. Legacy application source (MS Access forms, reports, queries and modules) already present in this workspace.

OUTPUT
- A modified copy saved as: FHN_Insurance_NET_Complete_Scope_16_Week_Development_Plan.pptx
- Do NOT overwrite the original file.
- Preserve the existing theme, colour palette (dark navy + teal), fonts, header bands, footer and slide numbers. Modify the existing slides; do not rebuild the deck from scratch.
- The final deck must look polished, consistent and client-ready.

==================================================
PHASE 1 – READ-ONLY DISCOVERY (no changes)
==================================================
1. Open the deck and list every shape, text frame and table on each slide with its current text.
2. Scan the legacy application and build an inventory of all forms/screens and reports/exports:
   - Legacy object name, business-friendly display name, business module, complexity (High / Medium / Low / Report-style export).
3. Reconcile the inventory against the deck counts: 45 screens (7 High / 14 Medium / 24 Low) and 29 reports (5 High / 12 Medium / 3 Low / 9 Report-style exports).
   - If your counts differ, do NOT change the deck numbers. Report the variance with object names.
4. Treat the legacy application as strictly read-only. Do not modify, move or delete any legacy file.

==================================================
PHASE 2 – GLOBAL RULES (apply to all slides)
==================================================
- Timeline is 16 weeks. Replace every reference to "4 months", "four-month", "Month 1–4" and "months" with the 16-week equivalent, including subtitles, labels and the footer.
- Hours must NOT appear anywhere in the deck: no hours column, no hours values, no hours in text boxes, headings, notes or footer.
- Replace the abbreviation "PD" with the full form "Person-days" everywhere.
- Total development effort is exactly 80 Person-days (16 weeks × 5 working days). Remove every reference to 87, 696 and "87 working days".
- Do not invent numbers or names. Keep all existing counts unchanged.
- Use professional, concise, client-facing technical English.

EFFORT BASELINE – 80 PERSON-DAYS (Count values unchanged)
- Screens – High (7): 14
- Screens – Medium (14): 14
- Screens – Low (24): 6
- Screens subtotal (45): 34
- Reports – High (5): 7.5
- Reports – Medium (12): 12
- Reports – Low (3): 1.5
- Report-style exports (9): 4.5
- Reports subtotal (29): 25.5
- Admin data grids (11): 2.75
- Shared .NET foundation (built once) (1): 7
- Integrations (6): 6
- Application integration + developer verification: 4.75
- TOTAL DEVELOPMENT: 80
Verify programmatically that line items sum to the subtotals and that all rows sum to 80.

==================================================
PHASE 3 – SLIDE CHANGES
==================================================

SLIDE 1 – Project Scope and 16-Week Development Roadmap
1. Header subtitle: "PROJECT SCOPE AND 16-WEEK DEVELOPMENT ROADMAP". Keep the title "FHN Insurance Legacy Application Modernization to .NET".
2. KPI card: "4 months / DEVELOPMENT TIMELINE" → "16 weeks / DEVELOPMENT TIMELINE".
3. Keep the complexity table and the "Also in Scope" panel unchanged.
4. Timeline label: "16-WEEK DEVELOPMENT ROADMAP – ONE FULL-STACK DEVELOPER, AI-ASSISTED (WINDSURF)".
5. Replace the four "Month 1–4" boxes with four phase boxes (content taken from the bottom month boxes currently on Slide 3), each with a caption "20 Person-days" under the header, broken down week by week:

   Weeks 1–4 | Foundation and Policies
   - Week 1: Shared .NET foundation – authentication, role-based access, navigation menu
   - Week 2: Reusable data grid, reporting engine, main menu and dashboard
   - Week 3: Reference data, admin data grids, Corporate Policies screens
   - Week 4: Corporate Policies completion, Policy Schedule reports

   Weeks 5–8 | Policy Reporting and Claims
   - Week 5: Remaining Policy reports
   - Week 6: Claim tasks, notes and attachments
   - Week 7: P&C Claims – view, add incident, add insured
   - Week 8: Workers' Compensation Claims – initial build

   Weeks 9–12 | Claims, Executive Risk and Integrations
   - Week 9: Workers' Compensation Claims, case management, payments
   - Week 10: Integrations – WC uploads (2), SharePoint, document management, SOV
   - Week 11: Executive Risk claims and payments
   - Week 12: FHN Properties, ATM, lease and attachments

   Weeks 13–16 | Specialty Lines, Reporting and Verification
   - Week 13: Property reports and exports
   - Week 14: OREO, BOLI, NFIP, Auto, D&O
   - Week 15: Tableau view and remaining reports
   - Week 16: End-to-end application integration and developer verification

   Keep the alternating navy/teal phase headers. Minimum font size 10 pt. Resize the KPI cards and complexity table proportionally if required so the slide stays clean and readable.

SLIDE 2 – Development Scope and Effort by Category
1. REMOVE the heading "Total development: 87 PD = 696 hours".
   New subtitle: "DEVELOPMENT SCOPE BY CATEGORY – REUSABLE COMPONENTS AND AI-ASSISTED DELIVERY"
   New title: "Development Scope and Effort by Category"
2. Table: REMOVE the "PD each" and "Hours" columns. Keep "Category", "Count", "Person-days". Apply the 80 Person-days baseline values from Phase 2 exactly.
3. Add a column "Screens / Reports in Scope" with business-friendly names from the Phase 1 legacy inventory, grouped by category (High / Medium / Low screens; High / Medium / Low reports; Report-style exports).
   - Use names derived from the legacy application only; do not fabricate names.
   - If the names do not fit, keep short module-level names on Slide 2 and add a new final slide "Appendix – Screen and Report Inventory" (table: Module | Screen / Report | Type | Complexity), split across multiple appendix slides if needed, same theme.
4. Right-side panels:
   - "REPORTS": "29 reports and exports delivered through a shared report engine and reusable report template. Effort is weighted by complexity: High, Medium, Low and report-style exports. Reports subtotal: 25.5 Person-days."
   - "HOW THE EFFORT FITS": retain existing text and add: "Total effort of 80 Person-days aligns to a 16-week plan (5 working days per week) for one full-stack developer."
   - "WHAT IS INCLUDED": bold lead labels — **Backend:** .NET APIs, data access, business rules. **Frontend:** all screens, grids and report views. **Quality:** integration, debugging, code review and unit tests. **Shared components:** built once and reused.

SLIDE 3 – Key Considerations (FYI)
1. DELETE the month-wise allocation table entirely.
2. DELETE the four month boxes and the bottom summary banner (this content now lives on Slide 1).
3. New subtitle "KEY CONSIDERATIONS – FYI"; new title "Assumptions, Dependencies and Open Items".
4. Add the following 8 points as numbered cards (two columns × four rows, teal number badges, bold lead phrase):
   1. **Business SME engagement from Day 1** – Business team support is required from project kick-off to validate business logic. Please nominate the business user / point of contact.
   2. **Scope confirmation – 20+ additional screens** – Current scope covers 45 screens. Our analysis identifies 20+ additional legacy screens currently out of scope; business confirmation is required on their inclusion.
   3. **Timeline excludes leave and holidays** – The 16-week plan is based on working weeks only and does not account for planned leave or public holidays.
   4. **Development scope only** – The plan covers solution setup and development. UAT, QA, DBA and DevOps activities are excluded from the 16-week estimate.
   5. **Production support priority** – Any defects or enhancements raised on BCAT, CPAT or CASRR will take priority and may impact the 16-week timeline.
   6. **Single-developer estimate** – The plan assumes one full-stack developer; additional developers can compress the overall timeline.
   7. **Progressive complexity discovery** – Deeper functional complexity is expected to surface during development; business user involvement is required to deliver as per business expectations.
   8. **QA strategy required** – There is currently no dedicated QA resource on the project; a QA testing approach (ownership, scope and timing) needs to be agreed.
5. Visually flag points 1, 2 and 8 as client action items (small "Action required" tag or teal accent border).

==================================================
PHASE 4 – VERIFICATION AND REPORT
==================================================
1. Programmatically search all slides, tables, text frames, speaker notes and footers and confirm zero occurrences of: "hour", "hours", "PD" (as abbreviation), "month", "Month", "87", "696".
2. Confirm the Slide 2 table sums to 80 Person-days and Slide 1 phases show 4 × 20 Person-days.
3. Confirm no text overflows its shape, no shapes overlap, and alignment and font sizes are consistent across slides.
4. Render each slide to PNG and review visually; fix any layout issue before finalising.
5. Report:
   - Changes per slide: ADDED / REMOVED / MODIFIED (shape name or table cell).
   - Legacy inventory summary and any count variance vs 45 screens / 29 reports, with object names.
   - Any item you could not complete and why.
6. Do not modify the legacy application. Do not commit or push.
