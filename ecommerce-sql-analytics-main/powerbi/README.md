# Power BI Dashboard Guide

## Star Schema Design

```
                    ┌─────────────┐
                    │  fact_sales │
                    │─────────────│
              ┌────▶│ order_id    │◀────┐
              │     │ customer_id │     │
              │     │ product_id  │     │
              │     │ seller_id   │     │
              │     │ revenue     │     │
              │     │ order_date  │     │
              │     └─────────────┘     │
              │                         │
    ┌─────────┴──┐              ┌───────┴─────┐
    │ dim_customer│              │ dim_product │
    └────────────┘              └─────────────┘
         │                            │
    ┌────┴──────┐              ┌──────┴──────┐
    │ dim_seller│              │  dim_date   │
    └───────────┘              └─────────────┘
```

## DAX Measures to Create

### Revenue KPIs
```dax
Total Revenue = SUM(fact_sales[revenue])

MoM Revenue Growth =
VAR CurrentMonth = [Total Revenue]
VAR PrevMonth = CALCULATE([Total Revenue], DATEADD(dim_date[date], -1, MONTH))
RETURN DIVIDE(CurrentMonth - PrevMonth, PrevMonth, 0)

YTD Revenue = TOTALYTD([Total Revenue], dim_date[date])
```

### Customer KPIs
```dax
Avg Order Value = DIVIDE([Total Revenue], DISTINCTCOUNT(fact_sales[order_id]))

Customer Retention Rate =
DIVIDE(
    CALCULATE(DISTINCTCOUNT(fact_sales[customer_id]),
              FILTER(fact_sales, fact_sales[order_count] > 1)),
    DISTINCTCOUNT(fact_sales[customer_id])
)
```

### Delivery KPIs
```dax
Late Delivery Rate =
DIVIDE(
    CALCULATE(COUNTROWS(fact_sales), fact_sales[is_late] = 1),
    COUNTROWS(fact_sales)
)
```