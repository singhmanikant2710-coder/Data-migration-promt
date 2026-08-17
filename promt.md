File: frontend/src/components/CrmSummaryTablePDF.tsx

Add textAlign: "center" to ONLY the four header <Text> cells for Bank PD, Bank LGD, 
CAS PD, CAS LGD (thBlue cells, cols6.c2 through cols6.c5):

    <Text style={[styles.thBlue, cols6.c2, { textAlign: "center" }]}>...</Text>
    <Text style={[styles.thBlue, cols6.c3, { textAlign: "center" }]}>...</Text>
    <Text style={[styles.thBlue, cols6.c4, { textAlign: "center" }]}>...</Text>
    <Text style={[styles.thBlue, cols6.c5, { textAlign: "center" }]}>...</Text>

Only these 4 header cells. Don't change body cells (td), column widths, or other 
headers. Only edit this file.
