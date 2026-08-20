READ-ONLY. Diagnostics only. Do NOT change anything. Do NOT edit any file.

STEP 1 — Locate the Covenant and Monitoring Information component:
grep -rl "Stepped Up Servicing\|Covenant and Monitoring\|Monitoring Covenants Accurately" frontend/src --include=*.tsx
grep -rl "covenant" frontend/src/app/review --include=*.tsx

Show file paths only.

STEP 2 — Once found, read that component ONCE. Findings only, no edits:

=== ISSUE 1 — Color coding (Is Stepped Up Servicing Required) ===
1. The "Is Stepped Up Servicing Required" dropdown. Show its JSX + any conditional color/className logic based on value. Currently is there any red/green styling? Geoff wants: "Yes" = red, "No" = green. Show current state.

=== ISSUE 2 — N/A saving as No ===
2. The three dropdowns: "Are Monitoring Covenants Accurately Tracked and Defined", "Are Covenants Accurately Calculated and Validated", "Are Covenant Breaches Adequately Addressed and Mitigated". Show each dropdown's JSX — its options (does it include "N/A"? What are the option values?), its value binding, and onChange.
3. The SAVE handler for this section — how these three values are collected and saved. Look specifically for any logic that converts/defaults "N/A" to "No" (e.g. a fallback, a boolean cast, `value || "No"`, or a mapping that loses "N/A"). Show the save/submit code.
4. What data type/values does each field expect — is it a boolean (Yes/No only, so N/A gets lost)? Or a string that supports "N/A"? This is likely why N/A → No.

Read once. Findings only. No edits. For Issue 2, I specifically need to find where N/A gets converted to No (save logic, boolean cast, or default fallback).
