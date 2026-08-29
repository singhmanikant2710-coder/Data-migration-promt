READ-ONLY. Read once, quote, stop. No loop. Find how the Month dropdown options are generated and why months after 202512 (2026) can't be selected.

SYMPTOM: For a ConsumerFinance (banking) customer, the Month dropdown only allows selecting up to 202512; months in 2026 (202601+) cannot be selected.

In frontend/src/app/blackbook/edit/page.tsx (and any month-selector component):
1) Find the Month dropdown / month selector (the control that sets selectedMonthKey). Quote its options source — where does the list of selectable months come from? Is it derived from series, rolling24, a fiscal-year filter, or an API call?
2) Is there any filter that limits months to a single fiscal year or caps at a certain monthKey? Quote it. Does it filter by intFiscalYear === selectedYear, or monthKey <= something, or slice a range?
3) How is the "latest"/default month determined? Quote it. Could a fiscal-year boundary (Dec 202512 → Jan 202601 in a new fiscal year) cause 2026 months to be excluded from the options?
4) When a month IS selected, quote the onChange — does selecting 202601 update selectedMonthKey, or is it blocked/ignored?

OUTPUT:
- A) The month options source, quoted.
- B) Any fiscal-year / monthKey cap filter, quoted.
- C) Whether 2026 months are excluded by a filter or simply absent from data.
- D) The likely reason 202512+ can't be selected: (i) options filtered to one fiscal year, (ii) monthKey cap, (iii) 2026 data absent, or (iv) onChange blocked.
- No fix. Findings only.
