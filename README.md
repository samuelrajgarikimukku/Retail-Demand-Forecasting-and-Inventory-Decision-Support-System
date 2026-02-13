Retail Demand Forecasting & Inventory Decision Support System
1. Project Overview

This project builds an end-to-end demand forecasting and inventory decision-support system using two years of retail transactional data (2022–2023, 50K+ records).

The objective is not only to predict product demand, but to translate forecasts into operational inventory decisions and quantify stock-out risk.

Core business question:

How much inventory should be stocked to avoid stock-outs without overstocking?

2. Dataset

Time Range: 2022-01-01 to 2023-12-31
Records: 50,000+ transactions
Granularity: Order-level (transactional)

Key columns used:

order_date

product_id

product_category

price

discount_percent

quantity_sold (target variable)

total_revenue

The target variable for forecasting is weekly quantity_sold.

3. Data Preparation

Converted order_date to datetime format.

Aggregated data to weekly SKU-level demand.

Selected top 15 SKUs by total sales volume.

Created a complete 105-week time-series panel per SKU.

Filled missing weeks with zero demand.

Forward-filled price and discount features for continuity.

This ensured clean time-series structure suitable for forecasting.

4. Demand Analysis

Key observations:

Each top SKU had sales in only 17–25 out of 105 weeks.

Coefficient of Variation (CV) for all SKUs > 2.

Demand was highly intermittent and volatile.

Average weekly demand was less than 1 unit for several SKUs.

This indicated forecasting complexity and high variability-driven inventory risk.

5. Forecasting Models
Baseline Model

Lag-based Linear Regression using:

lag_1

lag_2

lag_4

rolling_mean_4

Evaluation metrics:

MAE

RMSE

Safe MAPE (excluding zero-demand weeks)

Example (SKU 1087):

Naive Model:

MAE ≈ 1.71

MAPE ≈ 69%

Baseline Model:

MAE ≈ 1.12

MAPE ≈ 45%

The lag-based model significantly outperformed the naive benchmark.

Hybrid Model

Added business drivers:

weekly_avg_price

weekly_avg_discount_percent

Result:

Marginal improvement in MAE and RMSE, limited improvement in MAPE due to intermittent demand structure.

Insight: Price and discount were not strong drivers under sparse demand conditions.

6. Inventory Decision Logic

Assumptions:

Lead time = 2 weeks

Service level = 95%

Z = 1.65

Formulas:

Safety Stock = Z × σ × √Lead Time
Reorder Point = (Average Demand × Lead Time) + Safety Stock

Example (SKU 1087):

Mean weekly demand ≈ 0.76

Std deviation ≈ 1.74

Safety stock ≈ 4.06 units

Reorder point ≈ 5.58 units

Observation:

Variability dominated average demand, significantly inflating buffer inventory.

7. Service Level Sensitivity

Reorder Point Comparison:

90% service → ~4.67 units

95% service → ~5.58 units

99% service → ~7.25 units

Insight:

Inventory requirement increases nonlinearly as service level approaches 100%.
The last few percentage points of reliability are operationally expensive.

8. Stock-Out Risk Simulation

Simulated inventory consumption over the test period.

Logic:

If forecasted demand during lead time > available inventory → flag stock-out risk.

Example (SKU 1087):

3 risk weeks identified in test horizon

Estimated revenue at risk ≈ 7,948

This connects forecasting directly to financial exposure.

9. Key Business Insights

All top SKUs exhibited highly intermittent demand (CV > 2).

Lag-based models significantly outperform naive forecasting.

Price and discount features provided limited incremental value under sparse demand.

High variability increases safety stock disproportionately.

Increasing service level from 95% to 99% increased reorder point by ~30%.

Even low-average-demand SKUs can carry measurable revenue risk.

10. Limitations

Constant lead time assumed.

No real-time inventory data used.

No holding cost or optimization modeling included.

No external drivers such as holidays or macroeconomic variables.

Intermittent demand not modeled using specialized techniques (e.g., Croston’s method).

11. Skills Demonstrated

Time-series data engineering

Handling intermittent demand

Feature engineering (lag & rolling features)

Model benchmarking

Forecast evaluation using business-relevant metrics

Inventory control principles

Service-level trade-off analysis

Translating analytics into financial risk impact

12. Conclusion

This project demonstrates a full analytical workflow:

Data → Forecast → Inventory Decision → Risk Quantification → Business Impact

It showcases how forecasting models can be integrated into operational decision-making rather than treated as standalone predictive exercises.
