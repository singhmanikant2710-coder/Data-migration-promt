Update on the memo/linesheet header issue: I found the root cause. The table 
headers were set to "fixed", which in the PDF engine repeats them on every page 
of the whole document — this caused two problems: the header bleeding onto later 
unrelated sections, and the header appearing alone at the bottom of a page 
(orphaned) then repeating.

I've fixed the bleed and the orphan by tying the header to its table content. As 
a result, the header now appears once at the start of each table and won't 
repeat mid-page, won't orphan, and won't bleed into other sections. 

One trade-off: for a table that spans multiple pages, the header now shows once 
at the top of the table rather than repeating at the top of each continuation 
page. If you'd prefer the header to repeat cleanly on each page of a long table, 
that's possible but needs a different approach (splitting the table into 
dedicated pages) — let me know if that's important and I'll scope it.
