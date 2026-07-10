RESOLVED. Segment, Unit, and Market converted to cascading dropdowns (SEGMENT > UNIT > MARKET), sourced from live 01_DATA_01_Data Mart Trial.
- SEGMENT: distinct Segment values (independent).
- UNIT: cascades on Segment — Region values when Segment = 'Regional Banking' (case-insensitive), else SpecialtyLine values.
- MARKET: cascades on Segment + Unit — distinct Market values filtered accordingly.
Changing Segment clears/reloads Unit and Market; changing Unit clears/reloads Market. Bindings and Save path unchanged; selected values persist. Verified against live DB and tested on review 17836 (Segment=Wholesale).

RESOLVED. Portfolio Manager Lead, Executive Credit Officer, and Senior Credit Officer converted to dropdowns sourced from live 03_LIBRARY_10_Distribution Parties (display column Recipient_name).
- Portfolio Manager Lead: Recipient_role IN ('PML','RPML') — 16 names.
- Executive Credit Officer: Recipient_role = 'ECO' — 7 names.
- Senior Credit Officer: Recipient_role = 'SCO' — 16 names.
Option counts verified against DB. Bindings and Save path unchanged; selected values persist. Tested on review 17836.
