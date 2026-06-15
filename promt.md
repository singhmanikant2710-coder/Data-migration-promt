Create FILE 6:
frontend/src/services/api/naics.ts

Follow exact same pattern as:
frontend/src/services/api/selections.ts

Functions needed:

1. getNaicsItems(sector?: string)
   GET /api/v1/naics/library
   optional ?sector= filter

2. getNaicsItemByKey(key: string)
   GET /api/v1/naics/library/{key}

3. createNaicsItem(data: CreateNaicsDto)
   POST /api/v1/naics/library

4. updateNaicsItem(key: string, 
   data: UpdateNaicsDto)
   PUT /api/v1/naics/library/{key}

5. deleteNaicsItem(key: string)
   DELETE /api/v1/naics/library/{key}

6. getDistinctSectors()
   GET /api/v1/naics/sectors

7. getDistinctDivisions()
   GET /api/v1/naics/divisions

TypeScript interfaces needed:
- NaicsItem {
    naicsIndustryKey: string
    naicsDivision: string | null
    naicsSector: string | null
    naicsSubsector: string | null
    naicsIndustryGroup: string | null
    naicsCode: string | null
    naicsIndustryDescription: string | null
  }

- CreateNaicsDto {
    naicsIndustryKey: string
    naicsDivision?: string
    naicsSector?: string
    naicsSubsector?: string
    naicsIndustryGroup?: string
    naicsCode?: string
    naicsIndustryDescription?: string
  }

- UpdateNaicsDto {
    naicsDivision?: string
    naicsSector?: string
    naicsSubsector?: string
    naicsIndustryGroup?: string
    naicsCode?: string
    naicsIndustryDescription?: string
  }

Use exact same axios/fetch pattern as:
frontend/src/services/api/selections.ts

Confirm file path after creation.
Wait for approval before FILE 7.
