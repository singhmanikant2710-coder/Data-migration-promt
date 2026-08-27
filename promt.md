Apply the diff exactly as shown. Only change, in the perCashCollections block of backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs:
    LIKE 'PRINCIPAL'  ->  LIKE 'PRINCIPAL%'
(add the % wildcard). Nothing else changes. Apply now, then confirm the perCashCollections block uses LIKE 'PRINCIPAL%'.
