Single-file edit: frontend/src/components/pdf/CrmFindingsObservationsPDF.tsx

Fix (Item 1 — header): Make this report's header match the CRM Summary report: (a) fontSize 14 instead of 20, (b) header sits within page margins instead of full-page bleed, (c) header appears on page 1 only instead of repeating on every page.

Reference (CrmSummaryPDF.tsx headerBar): no absolute positioning, no `fixed`, sits in normal content flow with paddingVertical: 6, paddingHorizontal: 14, marginBottom.

Make these changes:

1. headerTitle fontSize: 20 -> 14
2. headerMeta fontSize: 20 -> 14
3. headerBar style — replace the absolute/full-bleed positioning with normal content-flow. 
   BEFORE (headerBar):
   {
     position: "absolute",
     top: 0,
     left: 0,
     right: 0,
     minHeight: 44,
     paddingVertical: 4,
     paddingLeft: MARGINS.left,
     paddingRight: MARGINS.right,
     backgroundColor: "#1F3864",
     display: "flex",
     flexDirection: "row",
     alignItems: "center",
     justifyContent: "space-between"
   }
   AFTER (headerBar):
   {
     paddingVertical: 6,
     paddingHorizontal: 14,
     marginBottom: spacing.lg + 4,
     backgroundColor: "#1F3864",
     display: "flex",
     flexDirection: "row",
     alignItems: "center",
     justifyContent: "space-between"
   }
4. Remove the `fixed` prop from the header View. 
   BEFORE: <View style={styles.headerBar} fixed>
   AFTER:  <View style={styles.headerBar}>
5. The page style currently has paddingTop: MARGINS.top + 10 to make room for the absolute header. Since the header is now in normal flow, change it to just paddingTop: MARGINS.top (remove the +10 that compensated for the floating header). Leave other page paddings (left/right/bottom) unchanged.
6. If a headerGap spacer View (height: 2) was only there to offset the fixed header, leave it for now unless it causes a visible gap — do NOT remove it in this edit (we can remove separately if needed).

CONSTRAINTS:
- Confirm `spacing` is already imported (it's used elsewhere). If spacing.lg is not available, use marginBottom: 14 instead.
- Do NOT change headerSubtitle, footer, tables, or any other style.
- Do NOT change any other file.
- Only edit this one file. Show the full diff.
