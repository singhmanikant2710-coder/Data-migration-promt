Also apply the same wrap={false} fix to Bug 216 (ScorecardResultsPDF.tsx), same pattern:
FILE: frontend/src/components/pdf/ScorecardResultsPDF.tsx
- Line ~401 (details header row): append wrap={false}
- Line ~411 (details data row): append wrap={false}
Same rules — no other changes.
