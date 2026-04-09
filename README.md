# FMCG-Supply-Chain-Analysis-Power-BI-Dashbaord
Data warehouse dashboard for FMCG delivery performance. Tracks On-Time, In-Full, OTIF metrics daily. Star schema with dim_customers, dim_products, dim_date, fact_order_lines &amp; fact_orders_aggregate. 

## Youtube Video Link: https://youtu.be/ZDX1OXGGaxE

## Dashboard

![dashboard](https://github.com/user-attachments/assets/9f66c032-86df-470d-9f34-e95422998e69)


## Data Model

<img width="1920" height="1080" alt="Screenshot (265)" src="https://github.com/user-attachments/assets/f89696ee-9e8b-4bda-b506-4499dd96abfa" />

## Key Measures Used

```
-- Delivery Performance
On Time Delivery % = DIVIDE([Orders Delivered On Time],[Total Orders],0)
In Full Delivery % = DIVIDE([Orders Delivered In Full],[Total Orders],0)
OTIF % = DIVIDE([Orders Delivered OTIF],[Total Orders],0)

-- Target Metrics
Avg On Time Target = AVERAGE(dim_targets_orders[ontime_target%]) / 100
Avg In Full Target = AVERAGE(dim_targets_orders[infull_target%]) / 100
Avg OTIF Target = AVERAGE(dim_targets_orders[otif_target%]) / 100

-- Performance Gaps
On Time Gap = [On Time Delivery %] - ([Avg On Time Target]/100)
In Full Gap = [In Full Delivery %] - ([Avg In Full Target]/100)
OTIF Gap = [OTIF %] - ([Avg OTIF Target]/100)

-- Order & Fulfillment Metrics
Orders Delivered On Time = CALCULATE(COUNTROWS(fact_orders_aggregate), VALUE(fact_orders_aggregate[on_time]) = 1)
Orders Delivered In Full = CALCULATE(COUNTROWS(fact_orders_aggregate), VALUE(fact_orders_aggregate[in_full]) = 1)
Orders Delivered OTIF = CALCULATE(COUNTROWS(fact_orders_aggregate), VALUE(fact_orders_aggregate[otif]) = 1)

Total Orders = COUNT(fact_order_lines[order_id])
Total Order Lines = COUNTROWS(fact_order_lines)
Total Order Lines Shipped = CALCULATE(COUNTROWS(fact_order_lines), fact_order_lines[actual_delivery_date] <> BLANK(), fact_order_lines[delivery_qty] > 0)
Fill Rates
Case Fill Rate = DIVIDE([Total Quantity Shipped],[Total Quantity Ordered],0)
Line Fill Rate = DIVIDE([Total Order Lines Shipped],[Total Order Lines],0)

-- Quantity Metrics
Total Quantity Ordered = SUM(fact_order_lines[order_qty])
Total Quantity Shipped = SUM(fact_order_lines[delivery_qty])

-- Performance Indicators
In Full Performance Icon =
VAR TargetValue = [Avg In Full Target]
VAR ActualValue = [On Time Delivery %]
RETURN
SWITCH(
TRUE(),
ActualValue > TargetValue, "▲", // Up arrow (Unicode)
ActualValue = TargetValue, "→", // Right arrow
ActualValue < TargetValue, "▼", // Down arrow
"─" // Default dash
)

```


## Key Findings

- OTIF Performance Gap — Overall OTIF at 16.13% significantly lags target of 65.91% (-49.78% gap), indicating critical supply chain efficiency issues
- In Full Delivery Underperformance — In Full Delivery at 29.33% vs target 76.51% (-47.18% gap) reveals consistent partial shipment challenges across network
- On Time Delivery Weakness — On Time Delivery at 32.80% vs target 86.09% (-53.29% gap), the most critical performance driver
- Fill Rate Optimization Opportunity — Line Fill Rate at 100% demonstrates perfect line fulfillment, but Case Fill Rate at 96.59% shows quantity shortfall opportunities
- Customer Segmentation — Expert Mart leads with 19.47% OTIF; Coolblue and Acclaimed Stores underperform significantly at 10.04% and 11.32% respectively
- Geographic Performance — Ahmedabad (16.49% OTIF), Surat (16.34% OTIF), and Vadodara (15.57% OTIF) show consistent performance, indicating systematic regional challenges
- Product Line Consistency — All AM products maintain 100% Line Fill Rate; performance bottlenecks are delivery timing and quantity fulfillment, not product availability
- Delivery Trends — Metric Performance Overtime shows declining trend from March to August, requiring immediate operational intervention


## Tools & Technologies

- Power BI (PL 300 Certified)


## License

This project is for portfolio and educational purposes. Data has been anonymized.

