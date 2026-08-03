Single-file edit: frontend/src/components/pdf/CroProductionSummaryPDF.tsx

The render prop does not fire in CRO footers, but it WORKS in CRM Summary 
(CrmSummaryPDF.tsx) on all pages. Copy CRM Summary's EXACT working footer 
structure into CRO — including the inner <View> wrapper that CRM uses.

First, show me CRM Summary's EXACT footer once more (verbatim) including the 
inner View and the fontSizes/spacing tokens it uses. Then replicate it 
EXACTLY in both CRO footers.

CRM's working footer pattern is (confirmed earlier):
  <View style={styles.footer} fixed>
    <View style={{ borderTopWidth: 1, borderTopColor: colors.divider, borderTopStyle: "solid", paddingTop: spacing.xs }}>
      <Text
        style={{ fontSize: fontSizes.small, color: "#475569", textAlign: "center" }}
        render={({ pageNumber, totalPages }) =>
          `${title} • Page ${pageNumber} of ${totalPages}`
        }
      />
    </View>
  </View>

Apply this EXACT structure to BOTH CRO footers, BUT:
- Check whether CRO's styles.footer matches CRM's styles.footer geometry 
  (CRM uses FOOTER_BOTTOM = 12, height 48, and styles.page paddingBottom = 
  FOOTER_BOTTOM + 56). If CRO's styles.footer/page differ, ALSO align CRO's 
  styles.footer and styles.page paddingBottom to match CRM's EXACT values, 
  since the working render behavior may depend on the footer geometry / page 
  padding.
- Use the CRO in-scope `title` (and `title`/appropriate var in the second 
  page).
- Ensure `fontSizes` and `spacing` tokens are imported in CRO (if CRM uses 
  fontSizes.small / spacing.xs, confirm CRO imports them; if not, use the 
  literal equivalents: fontSize 8, paddingTop 4).

Report:
1. CRM's exact footer + its styles.footer + FOOTER_BOTTOM + styles.page 
   paddingBottom (verbatim).
2. CRO's current styles.footer + styles.page paddingBottom.
3. Apply CRM's exact footer structure to both CRO footers and align 
   styles.footer/page geometry to CRM's.
4. Show both final CRO footer blocks and the updated styles.

Do NOT touch pageSetup.ts, page size, orientation, margins, header, or tables.
