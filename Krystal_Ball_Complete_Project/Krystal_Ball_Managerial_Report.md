# Krystal Ball — Hotel Bar Inventory Forecasting & Par-Level Recommendation

## 1. Core Business Problem & Operational Impact

Hotel bars face two competing inventory risks: **stockouts** of high-demand products and **overstocking** of slow-moving products. Stockouts can interrupt service and cause potential lost sales, while excessive inventory ties up working capital, consumes storage capacity, and increases exposure to shrinkage, breakage, and spoilage.

This project converts transaction-level bottle-balance records into regular daily demand series for each bar and brand. It then uses demand forecasting and a dynamic inventory policy to recommend par levels that account for expected lead-time demand and demand uncertainty.

## 2. Assumptions Made

- Supplier lead time is assumed to be **2 days**.
- Target service level is **95%**, corresponding to **Z = 1.645**.
- Unmet demand is treated as lost rather than backordered.
- Inventory is measured consistently in millilitres.
- Missing Bar × Brand × Date combinations represent zero recorded consumption.
- Safety stock is estimated from trailing demand variability.
- Lead-time demand variability is approximated using `sigma_daily × sqrt(L)`.
- The historical backtest uses a temporal holdout rather than randomized cross-validation.

The raw dataset contains **30,714 transactions**, 5 bars, and 10 brands covering **2026-01-01 to 2026-06-30**. The inventory conservation check passed for all rows.

## 3. Model Selection & Trade-Offs

Two transparent forecasting baselines are implemented: a **7-day moving average** and a **7-day seasonal-naive forecast**.

| Model | MAE | RMSE | WAPE |
|---|---:|---:|---:|
| naive7_pred | 1,662.45 | 2,989.37 | 79.85% |\n| ma7_pred | 1,428.73 | 2,226.26 | 68.62% |\n
The lower-WAPE baseline in this backtest is **ma7_pred**. This is a validation-period measurement, not a guarantee of future performance. A production implementation can compare Holt-Winters and tree-based models using rolling-origin backtesting.

## 4. Par Level, Performance & Future Improvements

The proposed policy uses:

`Lead-Time Demand = Forecast Daily Demand × L`

`Safety Stock = Z × sigma_L`

`Par Level = Lead-Time Demand + Safety Stock`

The simulation tracks stockout days, lost volume, average holding inventory, orders, and a turnover proxy.

Potential lost demand represented in the synthetic transaction dataset is **16,502,416 ml**. This allows the notebook to distinguish observed consumption from demand that could not be fulfilled.

Future improvements include promotional calendars, holiday/event indicators, live POS events, supplier lead-time distributions, minimum order quantities, bottle-size constraints, and explicit cost optimization.

## 5. Real-World Deployment & Production Considerations

A production workflow can run once per day after the previous day's POS data is finalized:

1. Ingest new POS and inventory transactions.
2. Validate conservation and data quality.
3. Update daily demand series.
4. Generate forecasts for each Bar × Brand.
5. Calculate safety stock and par levels.
6. Compare current inventory with reorder points.
7. Publish recommended order quantities to the bar-manager dashboard.
8. Monitor forecast error, data drift, stockouts, and inventory turnover.

Important failure modes include variable supplier lead times, holiday consumption shifts, promotions, new-product cold starts, missing POS events, and discrepancies caused by waste or breakage.

## Conclusion

The system converts transaction data into interpretable demand forecasts and dynamic inventory recommendations. The final policy should balance service reliability against the cost of holding excess stock rather than optimizing forecast accuracy alone.
