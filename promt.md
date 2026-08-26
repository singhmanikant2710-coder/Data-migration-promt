Use curCPLTD (with T) as the monthly source — the target TTM column is curCPLTDTTM and the codebase/existing SQL already use curCPLTD, so match that.

Before showing the diff, do ONE more read-only confirm: verify that a column named exactly curCPLTD exists in dbo.tblMain (via INFORMATION_SCHEMA or existing usage). If curCPLTD exists in tblMain, proceed and produce the unified diff. If it does NOT exist (only curCPLTDTTM exists as the aggregate but no monthly curCPLTD source), then STOP and tell me — in that case we simply omit the CPLTD sum from this method rather than guess a wrong source column.

All other mappings stay as instructed. Still add-only: do not modify or remove any existing code. Show the unified diff before applying.
