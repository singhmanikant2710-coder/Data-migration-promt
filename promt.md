CRM Summary Table Excel export issue — after clicking Export, a green "Report execution accepted" message appears, but no Save/download dialog ever appears afterward. Unclear if the file downloaded or not. READ-ONLY, no edits. One pass, answer, STOP.

1. Find the CRM Summary Table Excel export flow (frontend/src/app/reports/page.tsx or wherever "Export Excel" is wired for this report). Is it a synchronous download (blob → save immediately) or an async job (submit → poll/wait → download when ready)?
2. If async: what does "Report execution accepted" mean in this flow — is that a submission acknowledgment, after which the frontend is supposed to poll a status endpoint and then trigger a browser download? Find that polling/completion logic.
3. Is there a bug where the flow stops after showing "accepted" and never proceeds to the actual download step — e.g. a missing poll, a silently failing status check, or an error being swallowed?
4. Check the browser Network tab pattern this would produce: is there a follow-up request after "accepted" that should fetch the file? Does it fire at all?
5. Compare to how OTHER reports' Excel export works (if any other report exports to Excel successfully) — is CRM Summary Table using a different/newer export path that might be incomplete or broken?

Report the export flow, where it breaks (if it does), and whether this is a genuine bug or just missing UI feedback while a background job completes. Do NOT fix yet.
