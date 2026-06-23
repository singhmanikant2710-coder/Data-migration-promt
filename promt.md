READ-ONLY DIAGNOSTIC. Do NOT edit any file. Only read and report back.

Review History screen ka "Sample / Review Name" dropdown kis backend code se 
populate hota hai, wo trace karo. Frontend se shuru karo:
- frontend/src/app/review-history/page.tsx — dropdown kis service call se data leta hai
- us service call ka API endpoint
- us endpoint ka Controller + Service + Repository method

Mujhe batao:
1. Review History dropdown ka samples list banane wali repository method ka 
   poora SQL/LINQ query (jaisa hai waisa paste karo).
2. Kya wo 02_CORE_01_Samples ko 02_CORE_02_Reviews se join/filter karta hai? 
   Agar haan, join condition kya hai (Sample_id pe direct join hai kya)?
3. Review Status dropdown isi se alag kahan se bharta hai — woh source bhi batao.

Modify ONLY by reading. Do not touch any file. Agar koi file change zaroori 
lage to STOP karke poochho.
