READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/components/pdf/CrmSummaryPDF.tsx

Geoff wants several refinements to the Scorecard Assessment table. Show me ONLY (no edits):

=== Column headers (items 1, 2) ===
1. The Scorecard Assessment table header row JSX — the header cells for Scorecard ID, Date, Bank PD, Bank LGD, CAS PD, CAS LGD, Scorecard Type, Scorecard Assessment. Show each header cell's full style array and current textAlign.
2. The header cell style (th) and header text style (thText) definitions — current textAlign, and any height/padding.
3. Show exactly how "CAS PD" / "CAS LGD" / "BANK PD" / "BANK LGD" header text is rendered (are they single strings? Can I make "CAS PD" wrap to two lines?).

=== Values (item 3) ===
4. The VALUE cells for CAS PD and CAS LGD (data rows) — their current textAlign. Show the value cell JSX + styles.

=== Column widths (items 4, 5) ===
5. All column width classes sc1-sc8 with current % values (we previously set sc1=21.5%, sc3-6=7%/8%, sc8=21.5% etc — show ACTUAL current values). Confirm which sc maps to which column (Scorecard ID, Date, Bank PD, Bank LGD, CAS PD, CAS LGD, Type, Assessment).

=== Header height (item 6) ===
6. The th style padding/height. For reference, show another table's header (e.g. the findings table th) padding/height to see what "mirror other tables" means — what's the target smaller height.

Read once. Findings only. No edits. Flag shared styles.
