READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/app/reports/page.tsx

I saw the imports (many PDF components exist). Now show me ONLY (no edits):

1. The reports DROPDOWN options — where getReportNames() returns or where the dropdown list is defined. Show every report NAME the user can select, and the internal key/value/id for each.

2. The code that RUNS a selected report — the switch/if-else or mapping that takes the selected report and calls the right PDF component (e.g. CrmPdGradeMigrationDocument for PD Grade Migration). Show this mapping fully. I need to see: is the report matched by DISPLAY NAME (string) or by a stable KEY/ID?

3. Specifically for "PD/LGD Grade Migration": show the exact dropdown label and how it maps to CrmPdGradeMigrationDocument. If Geoff renames the display label to "PD Grade Migration", will it still run? (Depends on whether matching is by label or key.)

4. For ChecklistQuestionnaire: is ChecklistQuestionnairePDF fully wired (has a real query + data) or is it a placeholder/stub? Show how it's populated.

5. Which dropdown options, if any, have NO working implementation (map to nothing, or a stub)?

Read once. Findings only. No edits.
