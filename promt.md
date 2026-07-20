Backend only. Extend the Customer Info save path so RM and PM store BOTH name and number. Use LIVE DB, ignore columns.csv. Single file per edit. Do NOT modify or revert any existing logic authored by anyone (including Jothi) — only ADD to what exists. Every existing column's save behaviour must remain exactly as it is.

Context: the RM/PM dropdown values now arrive as a single string formatted "Number - Name" (e.g. "43724 - GEOFFREY HOULDITCH"). The DB columns are:
  dbo.[02_CORE_02_Reviews].[Relationship_mgr_name]   nvarchar
  dbo.[02_CORE_02_Reviews].[Relationship_mgr_number] int
  dbo.[02_CORE_02_Reviews].[Portfolio_mgr_name]      nvarchar
  dbo.[02_CORE_02_Reviews].[Portfolio_mgr_number]    int
Currently only the two name columns are written.

1) CustomerInfoFields model (find it — likely in Casrr.Application or Casrr.Domain)
   ADD four properties alongside the existing ones, following the same Has* pattern:
     int? RelationshipManagerNumber
     bool HasRelationshipManagerNumber
     int? PortfolioManagerNumber
     bool HasPortfolioManagerNumber
   Do not rename or remove any existing property.

2) backend/src/Casrr.Application/Services/ReviewService.cs — inside the existing "Persist Customer Info section" block
   After the existing relationshipManager / portfolioManager string is read, ADD parsing logic. Add a small local static helper:

     static (int? number, string? name) SplitNumberName(string? raw)
     {
         if (string.IsNullOrWhiteSpace(raw)) return (null, null);
         var idx = raw.IndexOf(" - ", StringComparison.Ordinal);
         if (idx <= 0) return (null, raw.Trim());          // no delimiter: treat whole value as the name
         var numPart = raw.Substring(0, idx).Trim();
         var namePart = raw.Substring(idx + 3).Trim();
         if (int.TryParse(numPart, out var n) && !string.IsNullOrWhiteSpace(namePart))
             return (n, namePart);
         return (null, raw.Trim());                        // not parseable: keep the raw value as the name
     }

   Then, ONLY when hasRelationshipManager is true, split the value and set:
     relationshipManager = parsed name (or original raw if unparseable)
     RelationshipManagerNumber = parsed number (may be null)
     HasRelationshipManagerNumber = hasRelationshipManager
   Do the same for portfolioManager -> PortfolioManagerNumber / HasPortfolioManagerNumber.

   IMPORTANT: the name column must receive ONLY the name (e.g. "GEOFFREY HOULDITCH"), never the "43724 - " prefix. If the value has no " - " delimiter (legacy CAS Users values), store it unchanged as the name and leave the number null — this preserves existing saved data behaviour.

   Set the new properties on the CustomerInfoFields object being built. Do not change any other field mapping.

3) backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs — SaveCustomerInfoAsync
   In the existing UPDATE statement, ADD two SET lines using the exact same CASE WHEN pattern as the surrounding lines:
     [Relationship_mgr_number] = CASE WHEN @hasRelationshipManagerNumber = 1 THEN @relationshipManagerNumber ELSE [Relationship_mgr_number] END,
     [Portfolio_mgr_number]    = CASE WHEN @hasPortfolioManagerNumber = 1 THEN @portfolioManagerNumber ELSE [Portfolio_mgr_number] END
   And ADD the four matching parameters in the same style as the existing ones:
     @relationshipManagerNumber  SqlDbType.Int   (DBNull when null)
     @hasRelationshipManagerNumber SqlDbType.Bit
     @portfolioManagerNumber     SqlDbType.Int   (DBNull when null)
     @hasPortfolioManagerNumber  SqlDbType.Bit
   Do not alter any existing SET line or parameter.

Do not touch the frontend in this step. Report the files changed.
