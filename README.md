# 🛒 Olist E-Commerce Analysis

BigQuery SQL project analyzing customer behavior, product performance, and revenue trends for **Olist**, Brazil's largest e-commerce marketplace.

---

## 🎯 Objective

Olist connects small Brazilian businesses to major marketplaces. This project explores the platform's data to answer three core business questions:

- Who are the customers, and how often do they buy?
- Which product categories drive the most sales and revenue?
- How have orders and revenue grown over time?

---

## 🗂️ Dataset

Six tables from BigQuery (`Olist` dataset):

| Table | Rows | Description |
|---|---|---|
| `olist_orders_dataset` | 99,441 | One row per order — status and timestamps |
| `olist_order_reviews_dataset` | 99,224 | Customer review scores per order |
| `olist_products_dataset` | 32,951 | Product catalog with categories |
| `olist_customers_dataset` | 99,441 | Customer location and unique ID |
| `olist_order_payments_dataset` | 103,886 | Payment type, installments, and value |
| `olist_order_items_dataset` | 112,650 | One row per item — product, seller, price |

---

## 🪜 Project Steps

### Case 1 — Customer Behavior Analysis

**Step 1:** Count unique customers and total orders on the platform.
→ 96,096 unique customers, 99,441 orders

**Step 2:** Calculate average orders per customer.
→ ~1.03 orders/customer — most customers buy only once

**Step 3:** Distribution of orders per customer (subquery).
→ 93,099 customers (96.9%) placed exactly 1 order

**Step 4:** Raw CLV table — total spending per customer (3-table JOIN).

**Step 5:** First KPI — Average Customer Lifetime Value.
→ ~166 per customer

**Step 6:** Customer segmentation using `CASE WHEN`.

| Segment | Condition | Count |
|---|---|---|
| Low Value | total_spent < 100 | 44,390 |
| Mid Value | total_spent 100–500 | 47,216 |
| High Value | total_spent > 500 | 4,489 |

**Step 7:** Revenue by segment.
→ High Value customers (5%) generate 26% of total revenue

---

### Case 2 — Product Performance Analysis

**Step 1:** Sales count by product category.
→ Top category: cama_mesa_banho (11,115 sales)

**Step 2:** Revenue by product category.
→ Top category: beleza_saude (1.26M) — high volume AND high revenue

**Step 3:** Average review score by category (all categories).

**Step 4:** Best reviewed categories filtered by `HAVING COUNT > 1000`.
→ Reliable insight: malas_acessorios (4.32 score, 1,088 sales)

---

### Case 3 — Order and Revenue Trend Analysis

**Step 1:** Monthly order count and total revenue using `FORMAT_TIMESTAMP`.
→ Platform grew steadily from 2016 to mid-2018

**Step 2:** AOV (Average Order Value) per month.
→ AOV declined as platform scaled — broader but lower-budget audience over time

---

## 📊 Key Findings

- **97% of customers placed only 1 order** — major retention problem
- Average customer lifetime value: **~166**
- **Top 5% of customers (High Value) generate 26% of revenue**
- `beleza_saude` is the platform's strongest category: high sales, high revenue, high satisfaction
- `relogios_presentes` has fewer sales but high-priced products → strong revenue
- `cama_mesa_banho` is the most sold category but NOT the highest revenue — volume ≠ revenue
- Platform revenue grew ~10x from early 2017 to late 2017
- AOV declined as order volume grew — classic marketplace growth pattern

---

## 🛠️ SQL Techniques Used

| Technique | Purpose |
|---|---|
| `COUNT(DISTINCT)` | Unique customer and order counts |
| `JOIN` (2–3 tables) | Linking orders, customers, payments, products |
| `GROUP BY` | Aggregation by category, month, segment |
| `HAVING` | Post-aggregation filtering (min sample size) |
| `CASE WHEN` | Customer segmentation |
| `Subquery` | Distribution analysis (orders per customer) |
| `AVG / SUM / COUNT` | KPI calculations |
| `FORMAT_TIMESTAMP` | Extracting year-month from timestamp |
| `ORDER BY DESC` | Ranking results |
| `LIMIT` | Sampling during exploration |

---

## 💾 Output Tables / Key Queries

```
Olist/
├── Customer behavior    ← Unique customers, CLV, segmentation
├── Product performance  ← Sales, revenue, review score by category
└── Revenue trend        ← Monthly orders, revenue, AOV
```

---

## 🔧 Tools

- **Google BigQuery** — SQL engine and data warehouse
- **SQL** — All analysis in standard SQL with BigQuery-specific functions
- **Dataset** — [Olist Brazilian E-Commerce (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
