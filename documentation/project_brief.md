# Olist Marketplace Performance Dashboard — Project Brief

**Client:** Ana Ferreira, Head of Marketplace Operations, Olist  
**Prepared for:** BI Analyst / Dashboard Developer  
**Date:** September 2026

## 1. Background
Olist connects small Brazilian retailers to major e-commerce marketplaces and handles logistics, payments, and order fulfillment. The business has approximately 100,000 orders from 2016–2018 but lacks a unified performance view. Reporting is performed manually from spreadsheets, making recurring reporting slow and error-prone.

The required solution is a **single source of truth dashboard** for operations, sales, and logistics teams.

## 2. Problem Statement
The project focuses on visibility into:
- Product categories and sellers driving revenue
- Delivery performance and missed estimates
- The relationship between delivery and customer satisfaction
- Low-review areas
- Payment behavior
- Geographic performance

## 3. Business Objectives
1. **Revenue & Growth** — sales trends by category, region, and seller.
2. **Delivery Performance** — missed estimates and delay magnitude.
3. **Customer Satisfaction** — review scores versus delivery delays, category, and payment behavior.
4. **Payment Behavior** — payment methods and installment patterns.
5. **Seller Performance** — revenue, delivery, orders, and reviews.
6. **Geography** — state/region performance.

## 4. Required Dashboard Pages
| Page | Must Show |
|---|---|
| Executive Summary | Revenue, orders, AOV, review score, YoY/MoM trend, KPIs |
| Sales & Product Performance | Category revenue, top/bottom products, price vs freight, trend |
| Delivery & Logistics | Actual vs estimated delivery, late %, state delivery time, delay vs reviews |
| Customer Satisfaction | Review distribution, review score vs delay, review score by category |
| Payments | Payment methods, installments, AOV by payment type |
| Geography | Orders/revenue by state, delivery time by state |
| Seller Performance | Revenue per seller, seller ranking, seller delivery reliability |

Seller Performance is a stretch goal.

## 5. Must-Have Features
- Date range filters
- State filters
- Product category filters
- Drill-through from summary to detail
- Clean, professional visual design

## 6. Expected Deliverables
1. Power BI `.pbix`
2. Published dashboard or screenshots
3. Documentation covering data sources, transformation, data model, DAX, and 3–5 business insights with recommendations
4. One-page stakeholder summary

## 7. Data Source
**Dataset:** Brazilian E-Commerce Public Dataset by Olist  
**Source:** Kaggle  
**License:** CC BY-NC-SA 4.0

Files:
- `olist_orders_dataset.csv`
- `olist_order_items_dataset.csv`
- `olist_products_dataset.csv`
- `olist_customers_dataset.csv`
- `olist_sellers_dataset.csv`
- `olist_order_payments_dataset.csv`
- `olist_order_reviews_dataset.csv`
- `olist_geolocation_dataset.csv`
- `product_category_name_translation.csv`

## 8. Success Criteria
- Leadership can understand performance quickly
- At least 3 actionable insights are surfaced
- Delivery delay vs review score is clearly visualized
- Dashboard is polished enough for external presentation

## 9. Suggested Timeline
| Phase | Task |
|---|---|
| Week 1 | Data cleaning, star schema modeling |
| Week 1–2 | DAX measures, initial visuals |
| Week 2 | Full report design, interactivity |
| Week 2–3 | Documentation, publishing, write-up |
