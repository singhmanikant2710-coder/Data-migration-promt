Two-file edit (backend logic + frontend type):
- backend/src/Casrr.Application/Reporting/CrmPdGradeMigration/CrmPdGradeMigrationReportService.cs
- frontend type (CrmPdGradeMigrationDetailRow) in BOTH:
  - frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx
  - frontend/src/services/api/reporting.ts

GOAL (Geoff): Fix the DETAIL table DIRECTION column logic/labels. 
pdInitial = Bank PD, pdFinal = CAS PD. Higher PD = worse credit = downgrade.
Correct rule:
- CAS PD (pdFinal) > Bank PD (pdInitial) -> "Downgrade"
- CAS PD (pdFinal) < Bank PD (pdInitial) -> "Upgrade"
- equal                                  -> "No Change"

Direction is DISPLAY-ONLY (matrices/totals/filters use numeric pdInitial/
pdFinal, not the Direction string), so this label change does not affect any 
data/calculations — confirmed by diagnostics.

=== BACKEND CHANGE (CrmPdGradeMigrationReportService.cs) ===
Current logic:
  string direction = "Unchanged";
  if (pdInit.HasValue && pdFinal.HasValue)
  {
      if (pdFinal.Value > pdInit.Value)
          direction = "Up";
      else if (pdFinal.Value < pdInit.Value)
          direction = "Down";
      else
          direction = "Unchanged";
  }

Change the labels to:
  string direction = "No Change";
  if (pdInit.HasValue && pdFinal.HasValue)
  {
      if (pdFinal.Value > pdInit.Value)
          direction = "Downgrade";
      else if (pdFinal.Value < pdInit.Value)
          direction = "Upgrade";
      else
          direction = "No Change";
  }

Do NOT touch the synthetic "No Grade Changes" display row — that special 
label stays as-is (it's a different case, used when there are no changes).

=== FRONTEND TYPE UPDATE ===
The detail row type currently declares:
  direction: "Up" | "Down" | "Unchanged";
Update it in BOTH files to:
  direction: "Downgrade" | "Upgrade" | "No Change" | "No Grade Changes";
(include "No Grade Changes" since the synthetic row uses it)

The frontend render (out(r.direction)) needs NO change — it displays whatever 
string comes from backend.

CONSTRAINTS:
- Only change the three direction LABEL strings in the backend logic 
  (Up->Downgrade, Down->Upgrade, Unchanged->No Change) and the default init 
  value ("Unchanged" -> "No Change").
- Do NOT change the comparison direction (pdFinal > pdInit stays as the 
  "Downgrade" branch — because higher CAS PD than Bank PD = downgrade).
- Do NOT touch matrices, totals, distributions, filters, or the 
  ExcludeUnchanged logic (they use numeric PDs, not Direction).
- Do NOT touch the "No Grade Changes" synthetic row.
- Update the TypeScript union type in both frontend files to match.
- Show the changed backend logic and both frontend type updates.
