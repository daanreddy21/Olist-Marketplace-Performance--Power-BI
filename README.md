# Olist Marketplace Performance Dashboard

## 📊 Power BI Business Intelligence Project

An interactive **Power BI dashboard** built using the Brazilian E-Commerce Public Dataset by Olist to analyze sales, product performance, delivery, customer satisfaction, geography, and seller performance.

---

## 📌 Project Overview

Olist connects small Brazilian retailers with major e-commerce marketplaces and manages logistics, payments, and order fulfillment.

The business had approximately 100,000 orders from 2016–2018 but lacked a unified performance view. Reporting was performed manually using spreadsheets, making recurring analysis slow and error-prone.

The goal of this project was to build a **single source of truth dashboard** that enables business users to analyze marketplace performance through interactive Power BI reports.

---

## 🎯 Business Problem

The business needed better visibility into:

- Which product categories generate the most revenue?
- Which sellers contribute the most sales and orders?
- Where are delivery delays occurring?
- How does delivery performance relate to customer satisfaction?
- Which payment methods and installment patterns are being used?
- Which Brazilian states have stronger or weaker performance?

The dashboard was designed to provide these answers through interactive visualizations and KPIs.

---

## 🎯 Business Objectives

### 1. Revenue & Growth

Analyze sales trends over time by:

- Product category
- Region/state
- Seller
- Year and month

### 2. Delivery Performance

Analyze:

- Actual vs estimated delivery time
- On-time delivery percentage
- Late orders
- Average delivery time
- Delivery performance by state

### 3. Customer Satisfaction

Analyze:

- Review score distribution
- Average review score
- Low-rated reviews
- Review score by category
- Review score vs delivery delay

### 4. Payment Behavior

Analyze:

- Payment methods
- Payment value
- Installments
- Average payment per order

### 5. Seller Performance

Analyze:

- Seller revenue
- Seller order volume
- Seller review performance
- Seller delivery performance

### 6. Geography

Analyze:

- Sales by state
- Orders by state
- Delivery time by state
- Geographic revenue distribution

---

# 🗂️ Dataset

## Brazilian E-Commerce Public Dataset by Olist

The project uses the Brazilian E-Commerce Public Dataset by Olist.

### Dataset Source

[Kaggle – Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

### License

The project brief identifies the dataset as **CC BY-NC-SA 4.0** and describes it as suitable for personal/portfolio use rather than commercial resale.

---

# 📁 Dataset Files

| Dataset | Purpose |
|---|---|
| `olist_orders_dataset.csv` | Order status and order lifecycle dates |
| `olist_order_items_dataset.csv` | Order items, products, sellers, price and freight |
| `olist_products_dataset.csv` | Product attributes and categories |
| `olist_customers_dataset.csv` | Customer information and state |
| `olist_sellers_dataset.csv` | Seller information and state |
| `olist_order_payments_dataset.csv` | Payment methods, values and installments |
| `olist_order_reviews_dataset.csv` | Customer reviews and review scores |
| `olist_geolocation_dataset.csv` | Geographic information |
| `product_category_name_translation.csv` | Portuguese-to-English category translation |

---

# 🏗️ Project Structure

```text
Olist-Marketplace-Performance/
│
├── README.md
│
├── data/
│   ├── raw/
│   │   ├── olist_orders_dataset.csv
│   │   ├── olist_order_items_dataset.csv
│   │   ├── olist_products_dataset.csv
│   │   ├── olist_customers_dataset.csv
│   │   ├── olist_sellers_dataset.csv
│   │   ├── olist_order_payments_dataset.csv
│   │   ├── olist_order_reviews_dataset.csv
│   │   ├── olist_geolocation_dataset.csv
│   │   └── product_category_name_translation.csv
│   │
│   └── processed/
│       └── README.md
│
├── powerbi/
│   └── Olist_Marketplace_Performance_Dashboard.pbix
│
├── screenshots/
│   ├── 01_executive_summary.png
│   ├── 02_sales_product_performance.png
│   ├── 03_delivery_logistics.png
│   ├── 04_customer_satisfaction.png
│   ├── 05_geography_analysis.png
│   └── 06_seller_performance.png
│
├── documentation/
│   ├── project_brief.md
│   ├── solution_report.pdf
│   ├── project_documentation.pdf
│   └── data_model.png
│
├── dax/
│   └── dax_measures.md
│
└── reports/
    └── stakeholder_summary.pdf