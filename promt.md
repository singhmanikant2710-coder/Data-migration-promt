Subject: PD Grade Migration — whitespace on page 1 (layout options)
Hi Geoff,
I looked into the whitespace below the distribution charts on page 1 of the PD Grade Migration report. Here's what's happening and a couple of options.
What's causing it: After the two charts, the next section is the "PD Grade Migration by Number of Accounts" matrix table. That table is kept together as a single block (so its header never gets separated from its data across a page break). On page 1, there isn't enough room left below the charts to fit the whole matrix, so the entire matrix moves to the next page — which leaves the space below the charts on page 1 empty. It's a natural result of keeping the table intact, not a data issue.
Two options:
Keep it as-is (current): The matrix stays together as one clean block on its own page. There's some whitespace after the charts on page 1, but every table remains fully intact and easy to read.
Reduce the whitespace: Let the matrix start right after the charts and flow across the page break. This fills the space on page 1, but the matrix table could then split across two pages (some rows on page 1, the rest on page 2), which can be harder to read.
My recommendation is option 1 — the current layout keeps the tables clean and intact, and a bit of whitespace before a large table is normal in reports like this. But it's your call. If you'd prefer I reduce the whitespace (option 2), I can do that, just with the trade-off that the table may split across pages.
Let me know which you'd prefer, or if the current layout is fine as-is.
Thanks!
Manikant
