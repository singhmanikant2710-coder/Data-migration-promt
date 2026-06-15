Fix the NAICS page header layout in:
frontend/src/app/maintenance/naics/page.tsx

PROBLEM:
When data loads, header breaks:
- Search bar moves above title
- "Filter by Sector" label on separate line
- Layout collapses

REQUIRED FIXED LAYOUT:

Header must ALWAYS be this structure:
┌─────────────────────────────────────────┐
│ NAICS          [Search] [Sector▼] [+Add]│
│ Library Maintenance                     │
└─────────────────────────────────────────┘

LEFT SIDE (never moves):
- "NAICS" bold large title
- "Library Maintenance" subtitle below

RIGHT SIDE (always one row, never wraps):
- Search input (fixed width w-48 
  flex-shrink-0)
- Sector dropdown (fixed width w-44 
  flex-shrink-0, no label above it,
  placeholder text "All Sectors" inside)
- "+ Add NAICS" button (flex-shrink-0)

FIX USING THESE EXACT CLASSES:

Header container:
className="flex items-start 
justify-between gap-4 mb-6"

Left side:
className="flex-shrink-0"

Right side:
className="flex items-center gap-2 
flex-shrink-0 flex-nowrap"

Search input:
className="w-48 flex-shrink-0 ..."

Sector dropdown:
className="w-44 flex-shrink-0 ..."

Add button:
className="flex-shrink-0 ..."

RULES:
- NO conditional classes based on data state
- NO state-dependent layout changes
- Same layout for 0 rows or 1378 rows
- Remove any label above Filter dropdown
- Dropdown placeholder = "All Sectors"
- Use flex-nowrap on right container

Only modify the header JSX section.
Do not change any logic or API calls.
Confirm after fix.
