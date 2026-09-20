# Krystal Ball — 3–5 Minute Video Presentation

## Slide 1 — Business Problem (0:00–0:45)
- Multiple hotel bars
- Stockouts of popular products
- Overstocking of slow-moving products
- Goal: maintain service levels while controlling inventory

**Say:** “The system predicts demand and converts that prediction into a dynamic par level.”

## Slide 2 — Data & Preprocessing (0:45–1:15)
- Transaction-level inventory records
- Timestamp parsing
- Conservation equation validation
- Daily Bar × Brand aggregation
- Explicit zero-demand days

**Show:** notebook data preview and daily consumption chart.

## Slide 3 — Forecasting (1:15–1:45)
- 7-day moving average
- 7-day seasonal naive
- Temporal validation
- MAE, RMSE, WAPE

**Show:** model comparison table and weekday demand chart.

## Slide 4 — Dynamic Par Level (1:45–2:30)
- Lead-time demand
- Safety stock
- 95% service level
- Par level = lead-time demand + safety stock

**Show:** formula and one Bar × Brand example.

## Slide 5 — Inventory Simulation (2:30–3:15)
- Daily consumption
- Reorder point
- Order up to par
- 2-day delivery delay
- Stockout and lost-volume tracking

**Show:** simulation output and inventory curve.

## Slide 6 — Results & Business Impact (3:15–4:00)
- Forecast WAPE
- Stockout days
- Lost volume
- Average holding inventory
- Orders / turnover proxy

**Say:** “The goal is not simply to maximize inventory; it is to balance availability with working capital.”

## Slide 7 — Production & Future Improvements (4:00–4:45)
- Daily automated pipeline
- Manager dashboard
- Supplier lead-time variability
- Holiday/event awareness
- Promotion integration
- Data drift monitoring
- Continuous forecast evaluation

## Closing (4:45–5:00)
“The final system turns transaction data into actionable reorder recommendations while making its assumptions and failure modes explicit.”
