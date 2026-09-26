Quote verbatim report/page.tsx:234-248 (the selectedYear fiscal
derivation). The verified backend formula is:
fy = (start == 1) ? y : (m >= start ? y + 1 : y)
Your report shows "m >= start ? y : y + 1". Confirm which one the code
uses; if it's the latter, fix it to match. Same report format.
