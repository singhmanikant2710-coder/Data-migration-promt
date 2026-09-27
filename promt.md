-- 1. Customer-level industry
SELECT strCustomerName, strIndustry
FROM tblCustomer WHERE strCustomerName LIKE 'AMERICAN CREDIT%';

-- 2. Month-level industry
SELECT strIndustry, COUNT(*) AS rows_, MIN(strMonthKey) AS fromMk, MAX(strMonthKey) AS toMk
FROM tblMain WHERE strCustomerName LIKE 'AMERICAN CREDIT%'
GROUP BY strIndustry;

-- 3. Kitne customers mein ye mismatch hai (generic impact)
SELECT m.strCustomerName, c.strIndustry AS customerIndustry,
       m.strIndustry AS monthIndustry, COUNT(*) AS rows_
FROM tblMain m JOIN tblCustomer c ON c.strCustomerName = m.strCustomerName
WHERE ISNULL(m.strIndustry,'') <> ISNULL(c.strIndustry,'')
GROUP BY m.strCustomerName, c.strIndustry, m.strIndustry
ORDER BY m.strCustomerName;


READ-ONLY. AMERICAN CREDIT ACCEPTANCE shows the wrong template:
extra Top Strip tiles "A/R $$$" and "A/R Turn Days"; missing "Avg
Principal N/R" (Month/TTM); Cash & Charge-offs shows "TTM Net C/O%"
where legacy shows "Net C/O TTM $" and "Net C/O TTM %"; missing
"Discount / Reserve $" and "Discount / Reserve %". Some of its tblMain
rows have strIndustry = NULL.

1. How does the new app choose the industry/template for a customer
   (URL param, tblMain.strIndustry, tblCustomer.strIndustry, fallback
   to generic)? Quote file:line for edit, view and report pages.
2. How does legacy choose it (frmMenu / tblCustomer.strIndustry ->
   which frm00X / rpt00X opens)? Quote the VBA/control source.
3. For the Indirect Auto legacy form, list the exact tile/panel labels
   and control sources for Month/TTM and Cash & Charge-offs; compare
   with our indirect-auto mapping and list differences.
Report only.
