READ-ONLY. Diagnostics only. Do not change anything.

The footer text `${title} • Page X of Y` is confirmed wired correctly (title 
is non-empty). But the footer still doesn't appear in the generated PDF. 
This is likely a positioning/rendering issue, not a title issue.

Show me:

1. The exact styles.footer definition (all properties: position, bottom, 
   left, right, height, paddingTop, border, etc.).

2. The value of MARGINS (from pageSetup) — specifically MARGINS.bottom — and 
   the styles.page paddingBottom. Does styles.page reserve enough bottom 
   padding for the footer height? If styles.page paddingBottom is smaller 
   than the footer height + MARGINS.bottom, the footer could be overlapping 
   content or pushed off/hidden.

3. Compare to the CRM Summary report (CrmSummaryPDF.tsx) which has a WORKING 
   footer: show ITS styles.footer, its FOOTER_BOTTOM/height, and its 
   styles.page paddingBottom. 

4. Specifically check: in CroProductionSummaryPDF, does styles.page have a 
   paddingBottom that accounts for the footer? The CRM Summary uses 
   paddingBottom: FOOTER_BOTTOM + 56. Does the CRO report have an equivalent 
   reservation, or is its footer positioned at bottom: MARGINS.bottom with a 
   height that may render BELOW the page's usable area / get clipped?

5. Confirm the footer <View> still has the `fixed` prop after the edit.

Do not edit anything. Compare the two files' footer positioning. Findings only.
