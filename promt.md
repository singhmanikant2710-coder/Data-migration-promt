Single-file edit: frontend/src/components/pdf/ScorecardResultsPDF.tsx

Fix (per Geoff): The Scorecard ID cell in the details table should show ONLY the Scorecard_id_bank value. Currently formatScorecardId returns both bank and system IDs (`${bank} ${system}`) when they differ, so the cell shows two IDs. Change it to always return only the bank ID.

BEFORE:
function formatScorecardId(row: ScorecardRow | any): string | null {
  const bank = (row?.scorecardIdBank ?? "").toString().trim();
  const system = (row?.scorecardIdSystem ?? "").toString().trim();

  if (bank && system && bank !== system) {
    return `${bank} ${system}`;
  }
  return bank || system || null;
}

AFTER:
function formatScorecardId(row: ScorecardRow | any): string | null {
  const bank = (row?.scorecardIdBank ?? "").toString().trim();
  return bank || null;
}

CONSTRAINTS:
- ONLY change formatScorecardId to return the bank ID only (never the system ID).
- Do NOT change buildGroups, the cell rendering, the de-dup logic, or anything else.
- Do NOT touch any other file.
- Show the diff.
