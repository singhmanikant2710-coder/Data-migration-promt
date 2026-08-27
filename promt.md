READ-ONLY. No edits. Find why 60+ DPD % (and similar) differ between the TOP STRIP (shows 0%) and the PANEL (shows 50% correct). Quote, stop. No loop.

SYMPTOM: In the Blackbook edit screen, several metrics show DIFFERENT values in the top summary strip vs the Month/Cash panels below. E.g. 60+ DPD % = 0% in the top strip but 50% in the Cash & Charge-offs panel. Same for TTM Net C/O % (top 2.89% vs panel 0.03%). The panel is correct; the top strip is stale/wrong.

In frontend/src/app/blackbook/edit/page.tsx:

1) Quote the monthlyTopStrip useMemo fully. It builds tiles via monthSummaryColumns...map(c => ({ value: c.render(lp) })). What is "lp"? Quote where lp is defined (latestPointComputed?). 

2) Quote latestPointComputed's definition. How is it built — from latestPoint, latestPointWithEdits, or seriesWithEdits? Does it run the tblMainCalcs client-side calcs (the for(const [key,fn] of Object.entries(tblMainCalcs)) loop) the same way seriesWithEdits does? Or does it use raw/merged values WITHOUT running the calcs?

3) Quote how the PANEL (MonthSummaryTable / the Cash & Charge-offs panel) gets its row for the selected month. Does it use seriesWithEdits (which runs the calcs) — so its values are correct?

4) Compare: the top strip renders c.render(lp) where lp=latestPointComputed. The panel renders from seriesWithEdits row. If latestPointComputed does NOT run the same tblMainCalcs calcs (or uses a different row that lacks curPrincipalNR/prior-month/merged TTM values), that's why the top strip shows 0/stale while the panel is correct.

OUTPUT:
- A) monthlyTopStrip + lp/latestPointComputed definitions, quoted.
- B) Does latestPointComputed run the tblMainCalcs calcs? yes/no + quoted.
- C) Does the panel use seriesWithEdits (calcs run)? yes/no + quoted.
- D) State plainly: the top strip is wrong because latestPointComputed [doesn't run the calcs / uses a different row without merged values], while the panel uses seriesWithEdits which does. Name the exact fix location (make latestPointComputed use the same computed row as seriesWithEdits for the selected month).
- No fix yet. Findings only.


READ-ONLY. Read the MapMetricPoint method in backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs. Read once, quote, stop. No loop.

Quote the part of MapMetricPoint where string fields are added to MetricPoint.Values — specifically where strCustomField* (or any str* selection fields) are placed into Values. I need to see the exact pattern used to add a string key to Values, so we can add the three selection fields the same way:
- strPrincipalOrGrossCalculationSelectionCashCollection
- strPrincipalOrGrossCalculationSelectionNetChargeOff
- strPrincipalOrGrossCalculationSelectionper60DPD

Also quote: how are these three selection columns read from the SQL reader (are they in the SELECT * / already available in the row), and what method adds a string to Values (e.g. AddStringValue, Values[...] = ...)? Quote the exact helper/pattern.

OUTPUT:
- The MapMetricPoint section adding string fields to Values (quoted).
- The exact pattern/helper to add a string key to Values.
- Confirm the three selection columns are available from tblMain (they're in the table).
- No fix yet. Findings only.



Edit ONLY backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs, only inside MapMetricPoint. Show unified diff before applying, then apply. Do not run build.

BUG: MapMetricPoint only preserves string columns whose name starts with "strcustomfield"/"customfield"/"custom". So the Principal-vs-Gross selection strings are NOT added to mp.Values, and the frontend calcs (perCashCollections etc.) read them as undefined and fall to the wrong branch (returning 0).

FIX: Extend the string-preservation condition to ALSO include the three selection fields (and any strPrincipalOrGross* selection).

Find this exact block:

                    var nlow = (name ?? string.Empty).ToLowerInvariant();
                    if (nlow.StartsWith("strcustomfield") || nlow.StartsWith("customfield") || nlow.StartsWith("custom"))
                    {
                        mp.Values[name] = s.Trim();
                    }

Replace it with:

                    var nlow = (name ?? string.Empty).ToLowerInvariant();
                    if (nlow.StartsWith("strcustomfield") || nlow.StartsWith("customfield") || nlow.StartsWith("custom")
                        || nlow.StartsWith("strprincipalorgross"))
                    {
                        mp.Values[name] = s.Trim();
                    }

The ONLY change: add `|| nlow.StartsWith("strprincipalorgross")` to the condition. This makes MapMetricPoint preserve strPrincipalOrGrossCalculationSelectionCashCollection, strPrincipalOrGrossCalculationSelectionNetChargeOff, and strPrincipalOrGrossCalculationSelectionper60DPD in mp.Values (all start with "strPrincipalOrGross", lowercased to "strprincipalorgross").

Do NOT change anything else. After applying, confirm the condition now includes strprincipalorgross and that mp.Values[name] = s.Trim() is used unchanged.
