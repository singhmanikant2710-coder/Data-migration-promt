Read-only. No edits. No plan. Just report with file paths + exact code. Do NOT modify anyone's work (including Jothi's).

Context (UAT #109): On Review Form → CRM Findings, the comments box must hide these toolbar buttons: Img URL, Upload, Table, +Row, -Row, +Col, -Col. This must apply ONLY to the CRM Findings comments field, not to other comment boxes (which are shared instances of the same editor and need those buttons for #65).

Report:
1) In CrmFindingsAndRatingsSection.tsx, the comments field JSX. Which editor component does it render (RichTextEditor?), and what props are passed to it? Paste that JSX.
2) In frontend/src/components/ui/RichTextEditor.tsx, paste the props interface/type and how the toolbar renders each of these buttons: Img URL, Upload, Table, +Row, -Row, +Col, -Col. Show the exact JSX for those buttons.
3) Confirm whether the same RichTextEditor is used by other comment fields (Customer Background, Covenants, Policy Exceptions, Scorecard, Risk Rating Justification) — list them, so I know a shared change would affect them.
4) State the cleanest way to hide those specific buttons for ONLY the CRM Findings instance via a new optional prop (defaulting to showing them everywhere else), and exactly which files/lines change.

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
