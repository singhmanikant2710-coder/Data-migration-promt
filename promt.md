READ-ONLY. Find how to expose the three Principal/Gross selector strings on the MetricPoint DTO so the frontend can seed principalGrossByLabel from the customer's ACTUAL stored basis. Quote with paths.

CONTEXT: 7-11% of customers use Gross N/R basis (925 cash, 1566 DPD). Currently the frontend dropdown defaults to "Principal N/R" (hardcoded), NOT the customer's stored selection. To make the Top Strip dropdown-follow work correctly (legacy parity) WITHOUT breaking Gross customers, we must seed the dropdown from the real stored basis. The backend reads these strings (SqlMainRepository.cs L4616) but drops them when building MetricPoint (numeric-only values bag).

Selector columns: strPrincipalOrGrossCalculationSelectionCashCollection, strPrincipalOrGrossCalculationSelectionper60DPD, strPrincipalOrGrossCalculationSelectionNetChargeOff.

1) Quote the MetricPoint DTO/model definition (backend) — where values (numeric bag) is defined. Is there a place to add string fields (e.g. a separate 'selections' dictionary, or top-level string properties)?
2) Quote MapMetricPoint (SqlMainRepository) — where it builds MetricPoint from the reader. How would we read the three selector strings and attach them (without polluting the numeric values bag)?
3) Quote the frontend MetricPoint type — how would the frontend receive these (e.g. point.selections?.cashCollection)?
4) Quote where principalGrossByLabel is seeded (edit/page.tsx L1908) — how to change the seed to use the row's selector (from the new DTO field) instead of the hardcoded default.
5) EXACT PLAN: (a) backend DTO — add a selections object/strings to MetricPoint; (b) MapMetricPoint — populate them; (c) frontend type — receive them; (d) seed principalGrossByLabel from the latest row's selections. Minimal, additive (new fields, no existing change).

OUTPUT:
- A) MetricPoint DTO — where to add selector strings (separate from numeric values), quoted.
- B) MapMetricPoint — how to read + attach them, quoted.
- C) Frontend MetricPoint type — how to receive, quoted.
- D) principalGrossByLabel seed — how to init from stored selection, quoted.
- E) Exact minimal additive plan (no existing behavior changed).
- No fix. Findings only.
