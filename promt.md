Help Tips are already working via a reusable HelpTipIcon component (or similar) — 
check how the Collateral tab integrates dynamic help tips from the help-tips library 
(API by form/topic), and replicate the EXACT same pattern on these review tabs:

- Key Risks
- CRM Findings
- CRM Ratings
- Risk Rating Justification
- Checklist

For each tab, use form "04_REVIEW FORM_04" and the correct topic that exists in the 
help-tips library. The topics available in the library are:
- Key Risks
- Risk Rating Justification
- Unsatisfactory Ratings  (use this for the CRM Ratings tab)
- Scorecard, Collateral, Covenants, Customer Info, Policy Exceptions, Regulatory Flags, 
  Repayment, Transactions, Sample Loading

Note: there is NO "CRM Findings" or "Checklist" topic in the library. For those two 
tabs, if no matching topic exists, either skip the help icon or leave the existing 
static one — do NOT invent a topic. Tell me which topic (if any) you used for CRM 
Findings and Checklist.

Replace any static tooltip on these tabs with the dynamic HelpTipIcon, matching the 
Collateral tab's usage exactly. Show me each file changed and the form/topic used per tab.

Modify only the relevant section components. If a shared file needs changing, STOP and ask.
