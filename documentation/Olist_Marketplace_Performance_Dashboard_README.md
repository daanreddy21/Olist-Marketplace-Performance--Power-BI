# Olist Marketplace Performance Dashboard

> **Power BI Business Intelligence Project**

## 1. Project Overview

The **Olist Marketplace Performance Dashboard** is an interactive Power
BI analytics project built using the **Brazilian E-Commerce Public
Dataset by Olist**.

The objective is to create a single analytical view of marketplace
performance across:

-   Revenue and sales growth
-   Product and category performance
-   Delivery and logistics
-   Customer satisfaction
-   Geography
-   Seller performance
-   Payment behavior at the data-model level

The dashboard is designed for marketplace operations, sales and
logistics stakeholders who need to understand performance without
relying on repeated manual spreadsheet reporting.

------------------------------------------------------------------------

## 2. Business Problem

Olist connects small Brazilian retailers to major e-commerce
marketplaces and handles logistics, payments and order fulfillment.

The project brief identifies a lack of a unified performance view.
Reporting was being performed manually from spreadsheets, making
recurring analysis slow and error-prone.

The key business questions are:

1.  Which product categories and sellers are driving revenue?
2.  Where is delivery performance breaking down?
3.  How does delivery performance relate to customer satisfaction?
4.  What payment methods and installment patterns are being used?
5.  Which Brazilian states show stronger or weaker marketplace
    performance?
6.  How are sellers performing in terms of revenue, orders and customer
    reviews?

------------------------------------------------------------------------

## 3. Business Objectives

### Revenue & Growth

Analyze sales trends over time and compare performance by category,
geography and seller.

### Delivery Performance

Identify delivery delays, compare actual and estimated delivery time,
and analyze state-level logistics performance.

### Customer Satisfaction

Analyze review-score distribution and examine the relationship between
delivery delay, product category and customer reviews.

### Payment Behavior

Analyze payment methods, payment transactions, payment value and
installment patterns.

### Seller Performance

Analyze seller revenue, order volume and review performance.

### Geography

Compare Brazilian states based on revenue, orders and delivery
performance.

------------------------------------------------------------------------

## 4. Dataset

### Dataset

**Brazilian E-Commerce Public Dataset by Olist**

### Source

Kaggle --- Brazilian E-Commerce Public Dataset by Olist

Source URL: https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

### License

The project brief identifies the dataset as **CC BY-NC-SA 4.0** and
describes it as suitable for personal/portfolio use rather than
commercial resale.

### Files Used

  -----------------------------------------------------------------------------
  File                                      Purpose
  ----------------------------------------- -----------------------------------
  `olist_orders_dataset.csv`                Order lifecycle, status and date
                                            information

  `olist_order_items_dataset.csv`           Order items, product, seller, price
                                            and freight

  `olist_products_dataset.csv`              Product attributes and product
                                            categories

  `olist_customers_dataset.csv`             Customer information and customer
                                            state

  `olist_sellers_dataset.csv`               Seller information and seller state

  `olist_order_payments_dataset.csv`        Payment methods, values and
                                            installments

  `olist_order_reviews_dataset.csv`         Customer reviews and review scores

  `olist_geolocation_dataset.csv`           ZIP-level geographic information

  `product_category_name_translation.csv`   Portuguese-to-English category
                                            translation
  -----------------------------------------------------------------------------

------------------------------------------------------------------------

## 5. Data Cleaning & Transformation

The data preparation process was performed before building the final
Power BI visuals.

### Duplicate Handling

Complete duplicate rows were considered for removal only when every
column was identical.

Repeated business keys were **not automatically treated as duplicates**,
because repeated keys can be valid according to the grain of each table.

For example:

-   An order can contain multiple order items.
-   An order can contain multiple payment records.
-   The same ZIP code can appear multiple times in the geolocation
    dataset.

### Orders

The orders dataset was cleaned and date columns were converted to
appropriate date/date-time types.

Important date fields include:

-   Order purchase date
-   Order approval date
-   Carrier delivery date
-   Customer delivery date
-   Estimated delivery date

### Order Items

The order-items table was treated at **order-item grain**.

The conceptual key is:

`order_id + order_item_id`

Repeated `order_id` values were retained because one order can contain
multiple products/items.

### Products

Product records were retained even when descriptive fields contained
blanks.

The original `lenght` column naming was standardized to `length` during
preparation.

Product category names were translated using the category translation
dataset.

### Payments

Payment records were kept at their transaction-level grain.

Repeated `order_id` values were retained because a single order can
contain multiple payment records.

### Reviews

Review records were kept separate from sales.

Blank review comments were treated as missing text rather than
automatically invalid records.

### Sales Fact

The project combines the order and order-item information into a sales
fact structure used for:

-   Sales
-   Orders
-   Quantity
-   Price
-   Freight
-   Delivery analysis
-   Product analysis
-   Seller analysis

### Separate Payment and Review Facts

Payments and reviews were kept separate because their grains differ from
the order-item sales fact.

This prevents accidental row multiplication when calculating sales,
payment and review metrics.

------------------------------------------------------------------------

## 6. Data Model

The report uses a dimensional relationship-based model.

### Main Model Structure

``` text
                    ┌───────────────┐
                    │   Date        │
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │  facts_sales  │
                    └───────────────┘
                       ▲     ▲    ▲
                       │     │    │
              ┌────────┘     │    └──────────┐
              │              │               │
       ┌──────┴──────┐ ┌────┴─────┐  ┌──────┴──────┐
       │  Products   │ │ Sellers  │  │    Orders   │
       └─────────────┘ └──────────┘  └──────┬──────┘
                                             │
                              ┌──────────────┼──────────────┐
                              ▼              ▼              ▼
                         Payments        Reviews       Customers
```

### Main Relationships

  ------------------------------------------------------------------------------------------------
  From                               To                                    Purpose
  ---------------------------------- ------------------------------------- -----------------------
  `Date[Date]`                       `facts_sales[purchase_date]`          Time analysis

  `products_dataset[product_id]`     `facts_sales[product_id]`             Product/category
                                                                           analysis

  `sellers_dataset[seller_id]`       `facts_sales[seller_id]`              Seller analysis

  `olist_orders_dataset[order_id]`   `facts_sales[order_id]`               Order-to-sales
                                                                           connection

  `olist_orders_dataset[order_id]`   `facts_payments[order_id]`            Order-to-payment
                                                                           connection

  `olist_orders_dataset[order_id]`   `facts_reviews_dataset[order_id]`     Order-to-review
                                                                           connection

  `customers_dataset[customer_id]`   `olist_orders_dataset[customer_id]`   Customer/location
                                                                           filtering
  ------------------------------------------------------------------------------------------------

### Date Table

A dedicated Date table was created for time intelligence.

Key columns include:

-   Date
-   Year
-   Month Number
-   Month Name
-   Month Short
-   Quarter
-   Quarter Number
-   Year Quarter
-   Year Month
-   Day
-   Day Name
-   Week Number
-   Is Weekend

Month names were sorted by Month Number so that months appear
chronologically.

------------------------------------------------------------------------

## 7. Key DAX Measures

### Sales Measures

``` dax
Total Sales =
SUM(facts_sales[price])
```

``` dax
Total Orders =
DISTINCTCOUNT(facts_sales[order_id])
```

``` dax
Total Quantity =
COUNTROWS(facts_sales)
```

### Average Order Value

``` dax
Average Order Value =
DIVIDE(
    [Total Sales],
    [Total Orders]
)
```

AOV is calculated using distinct orders rather than `AVERAGE(price)`
because the sales fact is at order-item grain.

### Time Intelligence

The project includes:

-   Sales YTD
-   Sales MTD
-   Sales QTD
-   Previous Month Sales
-   Sales Previous Year
-   Sales YoY Growth %

Example:

``` dax
Sales Previous Year =
CALCULATE(
    [Total Sales],
    SAMEPERIODLASTYEAR('Date'[Date])
)
```

### Delivery Measures

Key delivery measures include:

-   Average Actual Delivery Days
-   Average Estimated Delivery Days
-   On-Time Delivery %
-   Delivered Orders
-   Late Orders
-   Late Order %
-   Delivered Order %

### Customer Satisfaction Measures

Key measures include:

-   Average Review Score
-   Total Reviews
-   5 Star Reviews
-   4 Star Reviews
-   3 Star Reviews
-   2 Star Reviews
-   1 Star Reviews
-   Low Rating Reviews %

### Payment Measures

Key measures include:

-   Total Payment Value
-   Payment Transactions
-   Orders with Payment
-   Average Payment per Order
-   Average Payment Installments
-   Payment Transaction %

------------------------------------------------------------------------

## 8. Dashboard Pages

### Page 1 --- Executive Summary

The Executive Summary provides a high-level view of marketplace
performance.

Main elements:

-   Total Sales
-   Total Orders
-   Average Order Value
-   Average Review Score
-   On-Time Delivery %
-   Monthly Revenue Trend
-   Category Performance
-   Order Status
-   Sales by Year
-   Top Categories by Sales

### Page 2 --- Sales & Product Performance

Main elements:

-   Total Sales
-   Total Quantity
-   On-Time Delivery %
-   Delivered Orders
-   Average Delivery Days
-   Monthly Revenue Trend
-   Price vs Freight Cost
-   Top 5 Products/Categories by Revenue
-   Bottom 5 Products/Categories by Revenue
-   Sales Performance Matrix

### Page 3 --- Delivery & Logistics

Main elements:

-   Total Sales
-   Total Quantity
-   On-Time Delivery %
-   Delivered Orders
-   Late Orders
-   Average Delivery Days
-   Actual vs Estimated Delivery Time
-   Delivery Delay vs Average Review Score by State
-   Average Delivery Days by State
-   Late vs On-Time Delivery
-   Cancelled Orders by Product Category
-   Cancelled Orders by State

### Page 4 --- Customer Satisfaction

Main elements:

-   Total Quantity
-   Average Delivery Days
-   Average Review Score
-   Total Reviews
-   Low Rating Reviews %
-   Delivered Orders
-   Review Score Distribution
-   Low Rating Reviews by State
-   Delivery Delay vs Average Review Score
-   Average Review Score by Category

### Page 5 --- Geography Analysis

Main elements:

-   Total Sales
-   Total Quantity
-   Average Delivery Days
-   Average Review Score
-   Total Reviews
-   Sales by State
-   Orders by State
-   Revenue by State Map
-   Delivery Days by State

### Page 6 --- Seller Performance

Main elements:

-   Total Sales
-   Total Quantity
-   Average Delivery Days
-   Delivered Orders
-   Average Order Value
-   Sales by Seller
-   Orders by Seller
-   Seller Revenue vs Review Score

------------------------------------------------------------------------

## 9. Interactivity

The dashboard includes interactive slicers for:

-   Customer State
-   Product Category
-   Year / date context

A **Reset / Clear Slicer** control is included so users can return
filters to their default state.

The report also includes page navigation through the dashboard
navigation panel.

------------------------------------------------------------------------

## 10. Key Business Insights

### Insight 1 --- Revenue Concentration

Several categories contribute substantial revenue.

The dashboard displays approximately:

  Category                      Sales
  ------------------------- ---------
  Health & Beauty             \~1.26M
  Watches & Gifts             \~1.21M
  Bed, Bath & Table           \~1.04M
  Sports & Leisure            \~0.99M
  Computers & Accessories     \~0.91M

**Interpretation:** These categories represent important revenue areas
and should be monitored through category-level sales and
product-performance analysis.

### Insight 2 --- Geographic Variation in Delivery

Overall on-time delivery is **94.16%**, with approximately **96K
delivered orders** and **8K late orders**.

The state-level delivery chart shows substantial differences in average
delivery time, with the displayed values ranging from approximately **9
days in SP** to approximately **29 days in RR**.

**Interpretation:** Overall delivery performance can hide meaningful
geographic variation. State-level logistics analysis is therefore
important for identifying locations that require additional
investigation.

### Insight 3 --- Delivery and Customer Satisfaction

The dashboard contains a state-level scatter plot comparing:

-   Average Delivery Delay
-   Average Review Score

The visual shows variation across states in both measures.

**Interpretation:** Delivery performance and customer satisfaction
should be examined together. The dashboard provides evidence for further
investigation of this association, but the visual alone does not
establish that delivery delays directly cause lower review scores.

------------------------------------------------------------------------

## 11. Important Analytical Note

The Sales Performance Matrix contains a very large 2017 YoY percentage.

This occurs because the available 2016 data represents only a partial
period, while 2017 contains a much larger/full-year period.

Therefore, the displayed 2017 YoY percentage should **not** be
interpreted as a normal full-year growth rate.

For future reporting, a like-for-like period comparison would be more
appropriate.

------------------------------------------------------------------------

## 12. Tools & Technologies

### Data Analysis & BI

-   Microsoft Power BI
-   Power Query
-   DAX
-   Data Modeling

### Data

-   CSV
-   Olist Brazilian E-Commerce Dataset
-   Product Category Translation Dataset

### Analytical Techniques

-   Data cleaning
-   Data transformation
-   Dimensional modeling
-   KPI analysis
-   Time-series analysis
-   Geographic analysis
-   Customer satisfaction analysis
-   Seller performance analysis

------------------------------------------------------------------------

## 13. Project Deliverables

The project deliverables include:

1.  Power BI `.pbix` report
2.  Published Power BI report / screenshots
3.  Data model and DAX documentation
4.  Business insights
5.  Stakeholder summary
6.  Project documentation / README

------------------------------------------------------------------------

## 14. Limitations

-   The dataset represents historical Olist marketplace activity from
    2016--2018 and is not current marketplace performance.
-   Dashboard relationships describe patterns but do not establish
    causal relationships.
-   State-level metrics can be influenced by differences in order volume
    and missing records.
-   The Geography page map should be verified in the published Power BI
    environment because the supplied screenshot showed a Microsoft
    mapping/upgrade warning.
-   The original project brief specified a dedicated Payments page, but
    the final six-page dashboard screenshots do not show a separate
    Payments page.
-   Drill-through was included in the original requirements but was not
    verified from the final screenshots.

------------------------------------------------------------------------

## 15. Conclusion

The Olist Marketplace Performance Dashboard converts multiple e-commerce
datasets into an interactive business intelligence report.

The final report provides a consolidated view of:

-   Revenue
-   Orders
-   Product categories
-   Delivery performance
-   Customer reviews
-   Geography
-   Seller performance

The dashboard demonstrates the use of **Power Query, DAX, data modeling,
time intelligence and interactive Power BI visualization** to transform
raw transactional data into business-oriented analysis.

The three main analytical themes identified from the completed dashboard
are:

1.  Revenue concentration across leading product categories.
2.  Significant geographic variation in delivery performance.
3.  The need to analyze delivery performance alongside customer
    satisfaction.

------------------------------------------------------------------------

## 16. Portfolio / Resume Description

**Olist Marketplace Performance Dashboard \| Power BI**

Built an interactive Power BI dashboard using the Brazilian E-Commerce
Public Dataset by Olist to analyze revenue, orders, product categories,
delivery performance, customer reviews, geography and seller
performance. Performed data cleaning and transformation using Power
Query, designed a dimensional data model, developed DAX measures for
sales and time intelligence, delivery and customer satisfaction, and
implemented interactive slicers, navigation and reset functionality. The
dashboard provides KPI monitoring and business analysis of revenue
concentration, geographic delivery variation and the relationship
between delivery performance and customer reviews.

------------------------------------------------------------------------

## 17. Suggested Repository Structure

``` text
Olist-Marketplace-Performance/
│
├── README.md
│
├── data/
│   ├── olist_orders_dataset.csv
│   ├── olist_order_items_dataset.csv
│   ├── olist_products_dataset.csv
│   ├── olist_customers_dataset.csv
│   ├── olist_sellers_dataset.csv
│   ├── olist_order_payments_dataset.csv
│   ├── olist_order_reviews_dataset.csv
│   ├── olist_geolocation_dataset.csv
│   └── product_category_name_translation.csv
│
├── powerbi/
│   └── Olist_Marketplace_Performance_Dashboard.pbix
│
├── screenshots/
│   ├── executive-summary.png
│   ├── sales-product-performance.png
│   ├── delivery-logistics.png
│   ├── customer-satisfaction.png
│   ├── geography-analysis.png
│   └── seller-performance.png
│
└── documentation/
    ├── Olist_Marketplace_Performance_Dashboard_Documentation.pdf
    └── project-notes.md
```

## 18. Project Status

  Component                    Status
  ---------------------------- -------------------------------------------
  Data collection              Complete
  Data cleaning                Complete
  Data transformation          Complete
  Data modeling                Complete
  DAX measures                 Complete
  Dashboard design             Complete
  Reset filters                Complete
  Six dashboard pages          Complete
  Documentation                Complete
  Portfolio README             Complete
  Dedicated Payments page      Not present in final six-page screenshots
  Drill-through verification   Not verified
