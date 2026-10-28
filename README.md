# Final Project Outcome: Supply Chain Analytics, End to End

A portfolio project that builds a complete supply chain analytics stack, from the apps that create the transactions to a Power BI dashboard that turns them into actions. All data is synthetic.

## Presentation

- **[Open the presentation (PDF)](presentation/Supply_Chain_Analytics_End_to_End.pdf)**: 18 slides in two parts: how the project was built, and the dashboard story.
- **[Speaker notes](presentation/SPEAKER_NOTES.md)**: the talk track for every slide.

## What the project does

```
ERP app (Supabase)  ─┐
                     ├─> Python ETL (every 15 min, read-only) ─> Neon warehouse ─> Power BI dashboard
CRM app (Aiven)     ─┘                                        staging → dw → report schemas
```

1. **Source apps**: an ERP for purchase orders, receipts, inventory and sales orders across three distribution centers (KY, OH, TN), and a CRM for customers, forecasts and budgets.
2. **ETL**: incremental loads using row hashes, data-quality checks, and a web console to monitor runs.
3. **Warehouse**: a star schema plus report schemas (`rpt_inventory`, `rpt_demand`) that hold the business rules.
4. **Dashboard, "Distribution Network Health"**: five pages that trace a customer service problem back to late suppliers and end in a reorder list and a supplier action plan.

## Key findings (demo data)

| Measure | Value |
|---|---|
| Order fill rate | 96.7% (target 98%); October fell to 82.9% |
| SKU-locations at risk | 3 (2 stockouts, 1 reorder now) |
| Supplier on-time rate | 44.9% of received POs |
| Exception PO value | $75,212 across 66 POs; 9 in 10 are late deliveries |

Every number was reconciled from the source database through the warehouse to the dashboard.

## Related repositories

- [UniFirst_SoftwareDev_SCM](https://github.com/dharanig977/UniFirst_SoftwareDev_SCM): ERP app
- [UniFirst_CRM](https://github.com/dharanig977/UniFirst_CRM): CRM app
- [UniFirst_DataWarehouse](https://github.com/dharanig977/UniFirst_DataWarehouse): ETL, warehouse SQL and console

_Portfolio demo on synthetic data. Not a production system of any company._
