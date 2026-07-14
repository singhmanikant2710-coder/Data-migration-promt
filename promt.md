Frontend only. File: frontend/src/app/review-status/page.tsx. Single edit. Do not plan. Just apply.

The "Borrowers Sampled" status square is the only one that is not clickable. All the other six squares (Unopened/Cancelled, In Progress, Draft Completed, Approved, Distributed, Finalized) are clickable and set the Bucket filter.

Find how the squares are rendered. Report the root cause first (e.g. Borrowers Sampled is rendered as a separate JSX block outside the map, or it is excluded from the clickable list, or it has no onClick handler).

Then make it behave like the others: clicking "Borrowers Sampled" must set the Bucket filter to "All Statuses" and show all rows in the grid, using the same handler and the same hover/cursor-pointer styling as the other squares.

Do not change any count value, the SQL, the other squares, the grid columns, or pagination.
