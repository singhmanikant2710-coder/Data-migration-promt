READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

I need a STRUCTURAL orphan fix that works for ANY sample/data (not tuned to one PDF). A section heading gets separated from its table depending on data size. Show me ONLY (no edits):

1. ALL subreport/section components that render a sectionTitle heading followed by a table: Subreport01_Count, Subreport02_Commitment, Subreport03_DistByCount, Subreport04_DistByExposure. For EACH, show the JSX: how the heading and its table are wrapped (same parent View or separate siblings?), and whether any wrap={false} / minPresenceAhead / keep-together prop exists.

2. The matrices (MatrixCount, MatrixCommitment) heading+table structure too — same check.

3. Any EXISTING keep-together pattern in this file (e.g. trTotalsReserve uses minPresenceAhead — show its value and how it's applied).

4. The table sizes: are these distribution/subreport tables bounded (e.g. always ~14 PD rows) or can they grow large? This determines whether wrap={false} on the whole heading+table is safe, or if I need heading+first-row keep-together.

Read once. Findings only. No edits. I want one consistent orphan-control approach applied to all these heading+table sections so no sample breaks.
