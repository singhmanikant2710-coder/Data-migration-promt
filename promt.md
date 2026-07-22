Read-only. No edits. No plan. Just report with file paths + exact code. Do NOT modify anyone's work (including Jothi's).

Context (UAT #121): On the Review Status grid, the "Review Status" column for the Unopened/Cancelled bucket should show a per-row label:
  "Unopened"  when [Start_date] IS NULL AND [Cancelled] is No/False (0)
  "Cancelled" when [Cancelled] is Yes/True (1)
Currently it likely shows a single generic label for the whole bucket.

Report:
1) In frontend/src/app/review-status/page.tsx: how is the "Review Status" column value rendered for each row? What field does it read (e.g. r.reviewStatus / r.status)? Paste the header and cell JSX.
2) In backend/src/Casrr.Infrastructure/SqlServer/SqlReviewStatusRepository.cs, method GetUnopenedOrCancelledAsync: paste the full SELECT and the mapping. Does it already compute a per-row status label (e.g. a CASE expression returning 'Unopened' / 'Cancelled'), or does it return a fixed string? Show exactly what feeds the status column for these rows.
3) What does the ReviewStatusRow DTO field for the status hold, and how do the other buckets (In Progress, Approved, etc.) set their status text? Show one example so the pattern is clear.
4) State exactly what must change and in how many files to make the Unopened/Cancelled rows show "Unopened" (Start_date IS NULL AND Cancelled = 0) vs "Cancelled" (Cancelled = 1).

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
