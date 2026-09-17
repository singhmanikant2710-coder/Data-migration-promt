Yes, go ahead and start the backend half now in parallel:
- New Cassr.Application/Reporting/CrmSummaryForManagement/ service + contract
- New SqlCrmSummaryForManagementReportRepository with Finding_level = 'Finding'
  AND Finding_code <> 'CRM-00'
- New POST /api/v1/reports/crm-summary-for-management/execute on ReportsController
- Registered in StartupExtensions.AddDataProviders()

Base the contract on CrmSummaryResponse's existing fields for now. I understand that
if the prototype needs fields outside that payload, we'll extend the contract
afterward — that's an acceptable tradeoff to save a round trip.

Do NOT touch ICrmSummaryReportService, SqlCrmSummaryReportRepository, or any
existing endpoint — this must stay fully additive, same as agreed.

Hold off on the PDF component and the frontend routing/guard until I paste the
"Review Summary for Management" prototype structure — those depend on the layout.
