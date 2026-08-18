READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/components/pdf/ScorecardResultsPDF.tsx

The Scorecard ID value wraps badly — breaks char-by-char vertically (e.g. "27fd4955-" / "af90-" / ...). Show me ONLY (no edits):

1. The Scorecard ID VALUE cell JSX in the details table — how the id is rendered. Show the full <View>/<Text> and the formatScorecardId call. Does it insert \n or any breaks?
2. formatScorecardId definition — does it add "\n" or "\u200B" between bank/system IDs? (Earlier diagnostic showed it uses `${bank}\n${system}` when both differ.)
3. The Scorecard ID column width style (the sc/column class for this cell) — current width %.
4. The value cell's text style (tdText equivalent) — does it have wordBreak? Is this style shared with other columns?

Read once. Findings only. No edits.
