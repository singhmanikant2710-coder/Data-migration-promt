Master Prompt for Windsurf — Legacy Application Analysis & 4-Month Delivery Plan

Project Context

I have attached the following project assets to this workspace:

1. Legacy application source code and related project files — this is the application that needs to be analyzed and modernized/redeveloped.
2. Existing PowerPoint presentation (.pptx) — this presentation contains the original development plan with an estimated timeline of approximately 9 months.
3. Any supporting documentation, database files, screenshots, existing reports, business requirements, and other project artifacts available in the workspace.

Your task is to thoroughly analyze the actual legacy application and the existing PowerPoint presentation, then deliver TWO separate, professional PowerPoint presentations for client discussions.

The objective is to determine whether the application can realistically be delivered within 4 months, explain the effort required, and provide evidence-based justification for the proposed timeline.

Important: Do not simply compress the existing 9-month plan into 4 months by reducing estimates arbitrarily. Analyze the actual scope, complexity, dependencies, risks, available assets, parallel work opportunities, and AI-assisted development benefits before proposing a timeline.

---

DELIVERABLE 1: Update the Existing PPTX to a 4-Month Development Plan

Open the existing PowerPoint presentation and preserve its original purpose, project context, business requirements, and useful content.

Update the presentation to reflect a realistic 4-month delivery plan instead of the original 9-month estimate.

Required analysis

Before modifying the presentation:

1. Read and understand the complete existing PPTX, including its slides, diagrams, tables, timelines, assumptions, and scope.
2. Explore the legacy application's source code, architecture, modules, database structure, integrations, reports, and existing functionality.
3. Identify functionality that can be reused, functionality that needs modification, and functionality that requires new implementation.
4. Identify dependencies, technical blockers, business decisions, data migration requirements, testing needs, and external integration risks.
5. Determine which tasks can run in parallel and which tasks must be completed sequentially.
6. Compare the original 9-month plan against the proposed 4-month plan and explain the actual reasons for the difference.

Required updates to the existing PPTX

- Replace the 9-month timeline with a detailed 4-month implementation roadmap.
- Organize the roadmap into Month 1, Month 2, Month 3, and Month 4, with weekly milestones where appropriate.
- Include requirements validation, architecture/design, development, database work, integrations, report/PDF generation, testing, UAT, defect resolution, deployment, and handover wherever applicable.
- Show which workstreams can be executed in parallel.
- Include dependencies and critical-path activities.
- Identify deliverables and measurable exit criteria for each phase.
- Clearly distinguish development completion, QA completion, client UAT, production readiness, and production deployment.
- Include contingency for integration issues, legacy behavior discrepancies, data-quality issues, and UAT defects.
- Document assumptions about team size, developer availability, client feedback turnaround, environment readiness, and scope stability.
- Preserve existing branding, theme, useful diagrams, and relevant business content. Improve slide readability where needed.
- Remove obsolete timeline references and contradictory estimates throughout the presentation.

Timeline integrity

The 4-month plan must be supported by realistic effort estimates and a credible execution strategy.

If 4 months is achievable only with parallel development, early client decisions, scope prioritization, dedicated QA, or specific staffing assumptions, state those conditions explicitly.

If the analysis shows that the complete original scope cannot safely be delivered in 4 months, do not conceal that finding. Propose a clearly defined 4-month MVP or phased delivery approach and identify what would move to a later phase.

Deliverable 1 output

Save a new copy of the existing presentation as:

"FHN_CASSR_Development_Plan_4_Months.pptx"

Do not overwrite the original presentation.

---

DELIVERABLE 2: Create a New Client-Facing Complexity, Effort & Timeline Justification PPTX

Create a completely new, concise, professional PowerPoint presentation using the legacy application analysis, existing PPTX, and the revised 4-month roadmap.

This presentation must help us confidently answer the client's questions:

- How complex is the legacy application?
- Why will redevelopment take approximately 4 months?
- Why can it not simply be completed in a shorter period?
- If we use Windsurf, Claude, or other AI-assisted coding tools, why is significant development time still required?
- What exactly will consume the time?
- What evidence supports our estimates?
- What can be delivered in parallel, and what remains dependent on other activities?

The presentation should be suitable for a technical and non-technical client audience.

Section A: Application Scope Inventory

Inspect the entire legacy application and build a verified inventory of all screens, forms, modules, workflows, reports, and integrations.

The current preliminary scope is approximately:

- 45 screens/forms.
- 20 or more reports.

These numbers are preliminary estimates, not confirmed facts. Verify them against the actual application and existing documentation.

Create a consolidated inventory that includes:

- Screen/form or report name.
- Module or business area.
- Purpose and key business functionality.
- Relevant user actions and workflows.
- Database tables, stored procedures, queries, or APIs involved, where identifiable.
- Business rules and validations.
- Integrations and dependencies.
- Whether the functionality is reusable, requires modification, or requires new implementation.
- Complexity classification.
- Estimated implementation effort.
- Key risks, assumptions, and blockers.

Do not invent screen names, report names, business rules, or technical dependencies. Mark any information that cannot be verified as an assumption or an open question.

Section B: Complexity Classification

Classify every identified screen/form and report into one of three categories.

HIGH Complexity

Consider this category when evidence shows multiple interacting workflows, complicated business rules, complex calculations, substantial data relationships, integrations, security requirements, non-trivial state management, complicated report generation, or difficult legacy behavior.

MODERATE Complexity

Consider this category when a component has several validations, database interactions, business rules, workflow states, or moderate integration/reporting requirements.

LOW Complexity

Consider this category when a component is primarily a straightforward data-entry form, basic CRUD functionality, a simple lookup, a simple display screen, or a report with limited transformation and formatting.

These are starting guidelines, not fixed rules. Apply judgment based on the actual implementation.

For each classification, provide a short, evidence-based explanation. Do not classify components solely by their visual appearance or the number of fields on a screen.

Required summary

Include a summary table showing:

Category| Screens/Forms| Reports| Total Components| Estimated Effort
High| Verified count| Verified count| Calculated| Evidence-based estimate
Moderate| Verified count| Verified count| Calculated| Evidence-based estimate
Low| Verified count| Verified count| Calculated| Evidence-based estimate
Total| Verified count| Verified count| Calculated| Calculated

Do not fill this table with guessed numbers. Calculate it from the actual inventory.

Section C: Component-Level Effort Estimation

Estimate effort for each screen/form and report separately, then consolidate the estimates by module and complexity category.

Where applicable, separate effort into:

1. Requirements and legacy behavior analysis.
2. UI implementation.
3. Backend/API implementation.
4. Database changes, queries, and stored procedures.
5. Business logic and validation.
6. Integration and data handling.
7. Report/PDF layout and generation.
8. Unit and integration testing.
9. QA defect resolution and regression testing.
10. Deployment and documentation.

Do not automatically add every category to every component. Include only relevant work and account for shared implementation.

Use person-days or person-hours for estimates, and state the assumed working hours per day.

Clearly distinguish:

- Gross implementation effort.
- Reusable components and shared infrastructure.
- Effort saved through code reuse and AI-assisted development.
- Integration and cross-module effort.
- Testing and defect-fixing effort.
- Project coordination and client feedback dependencies.
- Contingency allowance.

Avoid double-counting shared components such as authentication, common layouts, reusable controls, database access layers, shared APIs, logging, and report-generation infrastructure.

Where exact effort cannot be established from the source code, provide a range and explain the uncertainty.

Section D: Why the Project Needs Approximately 4 Months

Build a defensible explanation based on actual project work rather than generic statements.

Show the relationship between:

- Total estimated engineering effort.
- Available development and QA capacity.
- Team size and actual allocation.
- Parallel workstreams.
- Dependencies and critical-path activities.
- Integration and end-to-end testing.
- Client review and UAT cycles.
- Defect correction and regression testing.
- Deployment readiness and contingency.

Provide a month-by-month and, where useful, week-by-week roadmap.

For each phase, include:

- Activities.
- Expected deliverables.
- Required roles.
- Dependencies.
- Estimated duration.
- Completion criteria.
- Major risks.

Explain why reducing the schedule further would require a specific trade-off, such as additional dedicated resources, reduced scope, deferred reports, fewer integrations, faster client decisions, or increased delivery risk.

Do not claim that 4 months is mathematically necessary unless the estimated effort, team capacity, and dependencies support that conclusion.

Section E: Address the Client's AI-Assisted Development Question

Include a dedicated slide titled:

"If We Use AI Coding Tools, Why Does Delivery Still Take 4 Months?"

Explain clearly that AI-assisted coding can accelerate code generation, refactoring, boilerplate creation, test drafting, documentation, and troubleshooting. However, it does not eliminate the need for engineering judgment, business-rule validation, architecture, integration, security, data correctness, testing, and acceptance.

Address these specific points:

- Understanding the legacy application's actual behavior.
- Discovering hidden business rules and undocumented dependencies.
- Mapping existing database relationships, stored procedures, and integrations.
- Translating legacy functionality into correct new implementations.
- Verifying AI-generated code against project conventions and requirements.
- Preventing regressions in existing functionality.
- Testing reports, calculations, PDF layouts, and edge cases.
- Validating permissions, security, and data integrity.
- Resolving environment-specific and integration issues.
- Performing end-to-end testing, regression testing, client UAT, and deployment.
- Handling defects and business feedback.

Quantify the expected AI productivity benefit only where defensible. Separate gross effort from AI-adjusted effort and explain the assumptions used.

Do not present an arbitrary percentage of AI productivity as an established fact. If there is no measured project-specific baseline, show an illustrative scenario or a range and label it clearly.

Emphasize that AI can reduce coding effort but cannot independently guarantee that the delivered application behaves correctly or meets client acceptance criteria.

Section F: Explain Why a Shorter Timeline Is Risky

Compare the proposed 4-month plan against a hypothetical shorter schedule, such as 3 months, only if useful and supported by the analysis.

Show what would happen to:

- Parallel staffing requirements.
- QA and regression coverage.
- Integration sequencing.
- Client feedback and UAT.
- Defect-fixing capacity.
- Deployment preparation.
- Risk of rework, missed functionality, and production defects.

Use a comparison table:

Delivery Area| 4-Month Plan| Shorter Timeline| Trade-off/Risk
Development| Evidence-based scope| Required acceleration| Explain
Integration| Planned validation| Compressed validation| Explain
QA/Regression| Planned coverage| Potentially reduced window| Explain
UAT| Defined feedback and retest cycles| Less time for iteration| Explain
Deployment| Planned readiness checks| Higher schedule pressure| Explain

Do not claim that a shorter schedule is impossible if it could be achieved with different resourcing, reduced scope, or other concrete changes.

Section G: Recommended Client Presentation Structure

Keep the new PPTX concise, ideally approximately 10–14 slides, using this structure:

1. Executive Summary.
2. Application Overview and Verified Scope.
3. Complexity Distribution: High, Moderate, and Low.
4. Screen/Form Complexity and Effort Summary.
5. Report Complexity and Effort Summary.
6. Representative High-Complexity Components and Evidence.
7. Overall Effort Breakdown and Estimation Methodology.
8. Four-Month Delivery Roadmap.
9. Team Capacity, Parallel Workstreams, and Dependencies.
10. Why AI-Assisted Development Does Not Eliminate Engineering Time.
11. Why a Shorter Timeline Increases Delivery Risk.
12. Key Assumptions, Risks, and Client Dependencies.
13. Deliverables and Acceptance Criteria.
14. Conclusion: Why the Proposed Timeline Is Defensible.

Combine or omit slides where necessary to keep the presentation concise and avoid repeating information.

The detailed component inventory and full effort breakdown may be included as appendix slides or a separate supporting spreadsheet if that makes the main presentation easier to understand. The main client presentation must remain concise.

Deliverable 2 output

Save the new presentation as:

"FHN_CASSR_Complexity_and_4_Month_Timeline_Justification.pptx"

If useful, also create:

"FHN_CASSR_Screen_Report_Effort_Inventory.xlsx"

The spreadsheet should contain the verified screen/form inventory, report inventory, complexity classifications, effort ranges, assumptions, and module-level totals.

---

Quality and Execution Requirements

1. Inspect the attached files and repository before making conclusions.
2. Do not modify production application source code as part of this planning task.
3. Do not overwrite the original PPTX or any source artifacts.
4. Use actual application evidence wherever possible.
5. Keep estimates transparent, internally consistent, and traceable to their assumptions.
6. Separate facts, estimates, assumptions, dependencies, and unresolved questions.
7. Validate that all component counts and effort totals reconcile across the inventory, spreadsheet, and presentations.
8. Ensure the 4-month timeline aligns with the total effort and realistically available team capacity.
9. Clearly state the assumed team composition and availability; do not invent dedicated developers, QA engineers, or other resources.
10. Account for shared infrastructure and reusable patterns without double-counting effort.
11. Use a professional, clean, client-ready design with readable text, clear tables, timeline visuals, and consistent formatting.
12. Avoid excessive technical jargon in the executive slides. Put deeper technical evidence in supporting slides or the appendix.
13. Do not fabricate savings, productivity percentages, project metrics, or claims about the legacy system.
14. If repository access, file parsing, or application analysis is incomplete, document the limitation instead of claiming complete analysis.
15. If the full application scope cannot credibly fit into 4 months, explicitly identify the required scope, staffing, or risk trade-offs rather than manipulating estimates.

Final Validation

Before finishing:

- Reopen and validate both generated PPTX files.
- Verify that slides render correctly and that no content is clipped, overlapping, or unreadable.
- Check that all slide titles, tables, dates, estimates, and totals are consistent.
- Confirm that the revised existing presentation reflects the 4-month plan throughout.
- Confirm that the new presentation explains the complexity, effort, AI-assisted development limitations, and timeline justification.
- Provide a concise completion summary listing the generated files, major findings, confirmed scope counts, total effort estimate, key assumptions, and any unresolved questions.

Start by inspecting all available attachments and the complete legacy application. Do not create the final presentations until the scope inventory, effort estimates, and 4-month feasibility analysis have been completed.
