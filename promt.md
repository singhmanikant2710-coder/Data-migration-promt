UAT #53 — final UI polish to match the client's Access prototype exactly.

Current state is close but two things are missing (see attached client screenshot):

1. COLUMN HEADERS inside the dropdown.
The Access prototype shows a header row INSIDE the open dropdown, above the options:
   Finding Code  |  Finding Description
Our dropdown has no header row. Add a sticky header row at the top of the option list (below the search box), with two labels aligned exactly over the two columns: "Finding Code" on the left, "Finding Description" on the right.
- The header must be sticky so it stays visible while scrolling the list.
- Style it distinctly from the option rows (e.g. slightly bolder text, subtle background, a bottom border) — but keep it consistent with the app's styling.

2. GRID / BOX APPEARANCE.
The Access dropdown looks like a bordered grid: a visible outer border around the whole list, and a light separator between the two columns and between rows.
Add to our dropdown:
- A clear outer border around the menu.
- A subtle vertical divider between the Finding Code column and the Finding Description column.
- Subtle horizontal row separators between options.
This should read as a small table/grid, exactly like the Access prototype.

Keep everything else as it is now — it is correct:
- Two columns, code left (fixed ~90px), description right, single line with ellipsis.
- Uniform row height.
- Search box at the top.
- Closed trigger shows ONLY the code.
- Option value stays the raw code — save path unchanged.

Apply and show me the diff.
