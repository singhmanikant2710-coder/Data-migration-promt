Create FILE 7:
frontend/src/app/maintenance/naics/page.tsx

Follow exact same PREMIUM UI pattern as:
frontend/src/app/maintenance/selections/
page.tsx

Page title: "NAICS"
Subtitle: "Library Maintenance"

FILTER SECTION (top right):
- Search input: "Search NAICS..."
- "Filter by Sector" dropdown
  (populated from getDistinctSectors())
- Default: "All Sectors"

STATS BAR:
- Total: XX records
- Sectors: XX (distinct count)
- Divisions: XX (distinct count)

TABLE COLUMNS:
Industry Key | Division | Sector | 
Subsector | Industry Group | 
NAICS Code | Description | Actions

INLINE ADD ROW:
- "+ Add NAICS" button (navy)
- Amber highlighted row at TOP
- Fields:
  • Industry Key (text input) — REQUIRED
  • Division (text input)
  • Sector (text input)
  • Subsector (text input)
  • Industry Group (text input)
  • NAICS Code (text input)
  • Description (text input)
- ✓ Save (green) | ✗ Cancel (gray)
- Validation: 
  Industry Key must be unique
  If duplicate → show error:
  "Industry Key '[key]' already exists."

INLINE EDIT ROW:
- Edit click → row becomes editable
- All fields editable except Industry Key
  (PK should not be editable)
- Save (green) | Cancel (gray)

ALL PREMIUM UI FEATURES:
- Dark navy header (#1F3864)
- Compact rows text-sm tracking-normal
- NO letter spacing on data cells
- Alternating row colors
- Smooth hover effect
- Read-only by default
- Pagination (10, 25, 50, 100)
- "Showing X-Y of Z results"
- Page number buttons
- Skeleton loader rows
- Delete modal popup (not browser alert)
  "Delete NAICS item?"
  Shows Industry Key being deleted
  Cancel | Delete (red)
- Toast notifications bottom-right
  auto-dismiss 3 seconds
- Empty state with message
- Responsive horizontal scroll

Import service from:
frontend/src/services/api/naics.ts

Keep all TypeScript interfaces.
Use Tailwind CSS only.

Confirm file path after creation.
Wait for approval before FILE 8.
