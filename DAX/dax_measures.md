# Olist Marketplace Performance Dashboard — DAX Measures

## Sales

```DAX
Total Sales =
SUM(facts_sales[price])
```

```DAX
Total Orders =
DISTINCTCOUNT(facts_sales[order_id])
```

```DAX
Total Quantity =
COUNTROWS(facts_sales)
```

```DAX
Total Freight =
SUM(facts_sales[freight_value])
```

```DAX
Average Order Value =
DIVIDE(
    [Total Sales],
    [Total Orders]
)
```

## Time Intelligence

```DAX
Sales YTD =
TOTALYTD([Total Sales], 'Date'[Date])
```

```DAX
Sales MTD =
TOTALMTD([Total Sales], 'Date'[Date])
```

```DAX
Sales QTD =
TOTALQTD([Total Sales], 'Date'[Date])
```

```DAX
Previous Month Sales =
CALCULATE([Total Sales], PREVIOUSMONTH('Date'[Date]))
```

```DAX
Sales Growth % =
DIVIDE(
    [Total Sales] - [Previous Month Sales],
    [Previous Month Sales]
)
```

```DAX
Sales Previous Year =
CALCULATE(
    [Total Sales],
    SAMEPERIODLASTYEAR('Date'[Date])
)
```

```DAX
Sales YoY Growth % =
DIVIDE(
    [Total Sales] - [Sales Previous Year],
    [Sales Previous Year]
)
```

## Delivery

```DAX
Average Actual Delivery Days =
AVERAGEX(
    FILTER(
        VALUES(facts_sales[order_id]),
        NOT ISBLANK(CALCULATE(MAX(facts_sales[delivered_date])))
    ),
    DATEDIFF(
        CALCULATE(MIN(facts_sales[purchase_date])),
        CALCULATE(MAX(facts_sales[delivered_date])),
        DAY
    )
)
```

```DAX
Average Estimated Delivery Days =
AVERAGEX(
    FILTER(
        VALUES(facts_sales[order_id]),
        NOT ISBLANK(CALCULATE(MAX(facts_sales[estimated_delivery_date])))
    ),
    DATEDIFF(
        CALCULATE(MIN(facts_sales[purchase_date])),
        CALCULATE(MAX(facts_sales[estimated_delivery_date])),
        DAY
    )
)
```

```DAX
On-Time Delivery % =
DIVIDE(
    CALCULATE(
        DISTINCTCOUNT(facts_sales[order_id]),
        facts_sales[delivered_date] <= facts_sales[estimated_delivery_date]
    ),
    CALCULATE(
        DISTINCTCOUNT(facts_sales[order_id]),
        NOT ISBLANK(facts_sales[delivered_date])
    )
)
```

```DAX
Average Delivery Delay =
AVERAGEX(
    FILTER(facts_sales, NOT ISBLANK(facts_sales[delivered_date])),
    DATEDIFF(
        facts_sales[estimated_delivery_date],
        facts_sales[delivered_date],
        DAY
    )
)
```

```DAX
Delivered Orders =
CALCULATE(
    DISTINCTCOUNT(facts_sales[order_id]),
    facts_sales[order_status] = "delivered"
)
```

```DAX
Cancelled Orders =
CALCULATE(
    DISTINCTCOUNT(facts_sales[order_id]),
    facts_sales[order_status] = "canceled"
)
```

```DAX
Late Orders =
CALCULATE(
    DISTINCTCOUNT(facts_sales[order_id]),
    FILTER(
        facts_sales,
        NOT ISBLANK(facts_sales[delivered_date])
            &&
        facts_sales[delivered_date] > facts_sales[estimated_delivery_date]
    )
)
```

```DAX
Late Order % =
DIVIDE([Late Orders], [Delivered Orders])
```

```DAX
Delivered Order % =
DIVIDE([Delivered Orders], [Total Orders])
```

## Delivery Status Column

```DAX
Delivery Status =
IF(
    ISBLANK(facts_sales[delivered_date]),
    BLANK(),
    IF(
        facts_sales[delivered_date] <= facts_sales[estimated_delivery_date],
        "On Time",
        "Late"
    )
)
```

## Customer Satisfaction

```DAX
Average Review Score =
AVERAGE(facts_reviews_dataset[review_score])
```

```DAX
Total Reviews =
DISTINCTCOUNT(facts_reviews_dataset[review_id])
```

```DAX
5 Star Reviews =
CALCULATE([Total Reviews], facts_reviews_dataset[review_score] = 5)
```

```DAX
4 Star Reviews =
CALCULATE([Total Reviews], facts_reviews_dataset[review_score] = 4)
```

```DAX
3 Star Reviews =
CALCULATE([Total Reviews], facts_reviews_dataset[review_score] = 3)
```

```DAX
2 Star Reviews =
CALCULATE([Total Reviews], facts_reviews_dataset[review_score] = 2)
```

```DAX
1 Star Reviews =
CALCULATE([Total Reviews], facts_reviews_dataset[review_score] = 1)
```

```DAX
Low Rating Reviews % =
DIVIDE(
    [1 Star Reviews] + [2 Star Reviews],
    [Total Reviews]
)
```

### Category Review Score

```DAX
Average Review Score by Category =
CALCULATE(
    [Average Review Score],
    TREATAS(
        VALUES(facts_sales[order_id]),
        facts_reviews_dataset[order_id]
    )
)
```

## Payments

```DAX
Total Payment Value =
SUM(facts_payments[payment_value])
```

```DAX
Payment Transactions =
COUNTROWS(facts_payments)
```

```DAX
Orders with Payment =
DISTINCTCOUNT(facts_payments[order_id])
```

```DAX
Average Payment per Order =
DIVIDE(
    [Total Payment Value],
    [Orders with Payment]
)
```

```DAX
Average Payment Installments =
AVERAGE(facts_payments[payment_installments])
```

```DAX
Payment Transaction % =
DIVIDE(
    [Payment Transactions],
    CALCULATE(
        [Payment Transactions],
        ALL(facts_payments[payment_type])
    )
)
```

## Other

```DAX
Average Items per Order =
DIVIDE(
    [Total Quantity],
    [Total Orders]
)
```

## Date Table

```DAX
Date =
CALENDAR(
    MIN(facts_sales[purchase_date]),
    MAX(facts_sales[purchase_date])
)
```

```DAX
Year = YEAR('Date'[Date])
Month Number = MONTH('Date'[Date])
Month Name = FORMAT('Date'[Date], "MMMM")
Month Short = FORMAT('Date'[Date], "MMM")
Quarter = "Q" & QUARTER('Date'[Date])
Quarter Number = QUARTER('Date'[Date])
Year Quarter = 'Date'[Year] & "-" & 'Date'[Quarter]
Year Month = FORMAT('Date'[Date], "YYYY-MM")
Day = DAY('Date'[Date])
Day Name = FORMAT('Date'[Date], "DDDD")
Day Short = FORMAT('Date'[Date], "DDD")
Week Number = WEEKNUM('Date'[Date], 2)
Is Weekend = IF(WEEKDAY('Date'[Date], 2) >= 6, "Yes", "No")
```

## DAX Notes

- AOV is calculated using total sales / distinct orders because the sales fact is at order-item grain.
- Time-intelligence measures depend on the dedicated Date table.
- The category-review measure uses `TREATAS` to transfer selected order IDs to the review table.
- The very high 2017 YoY value should not be treated as a normal full-year comparison because the 2016 period is incomplete.
