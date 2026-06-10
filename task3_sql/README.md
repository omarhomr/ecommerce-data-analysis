# Task 3 — SQL Queries for Data Analysis

## Table Schema

```sql
CREATE TABLE orders (
  OrderID        VARCHAR(20)    PRIMARY KEY,
  OrderDate      DATE,
  CustomerID     VARCHAR(10),
  Product        VARCHAR(50),
  Quantity       INT,
  UnitPrice      DECIMAL(10,2),
  ShippingAddr   VARCHAR(100),
  PaymentMethod  VARCHAR(20),
  OrderStatus    VARCHAR(20),
  TrackingNumber VARCHAR(20),
  ItemsInCart    INT,
  CouponCode     VARCHAR(20)    DEFAULT 'No Coupon',
  ReferralSource VARCHAR(30),
  TotalPrice     DECIMAL(10,2)
);
```

---

## Query 1 — Revenue & Orders by Product

```sql
SELECT
  Product,
  COUNT(*)                   AS total_orders,
  SUM(TotalPrice)            AS total_revenue,
  ROUND(AVG(TotalPrice), 2)  AS avg_order_value
FROM orders
GROUP BY Product
ORDER BY total_revenue DESC;
```

---

## Query 2 — Order Status with Percentages

```sql
SELECT
  OrderStatus,
  COUNT(*) AS count,
  ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM orders), 1) AS pct
FROM orders
GROUP BY OrderStatus
ORDER BY count DESC;
```

---

## Query 3 — Monthly Revenue Trend

```sql
SELECT
  DATE_FORMAT(OrderDate, '%Y-%m') AS month,
  COUNT(*)                          AS orders,
  ROUND(SUM(TotalPrice), 2)         AS revenue
FROM orders
GROUP BY DATE_FORMAT(OrderDate, '%Y-%m')
ORDER BY month;
```

---

## Query 4 — Top 10 Customers (by Lifetime Value)

```sql
SELECT
  CustomerID,
  COUNT(*)                   AS total_orders,
  ROUND(SUM(TotalPrice), 2)  AS lifetime_value
FROM orders
WHERE OrderStatus NOT IN ('Cancelled', 'Returned')
GROUP BY CustomerID
ORDER BY lifetime_value DESC
LIMIT 10;
```

---

## Query 5 — Coupon Effectiveness

```sql
SELECT
  CouponCode,
  COUNT(*)                   AS usage,
  ROUND(AVG(TotalPrice), 2)  AS avg_order,
  ROUND(AVG(Quantity), 2)    AS avg_qty
FROM orders
GROUP BY CouponCode
ORDER BY avg_order DESC;
```

---

## Query 6 — Referral Sources vs Revenue

```sql
SELECT
  ReferralSource,
  COUNT(*)                   AS orders,
  ROUND(SUM(TotalPrice), 2)  AS revenue,
  ROUND(AVG(TotalPrice), 2)  AS avg_value
FROM orders
GROUP BY ReferralSource
ORDER BY revenue DESC;
```

---

## Query 7 — Cancellation Rate by Product

```sql
SELECT
  Product,
  COUNT(*) AS total_orders,
  SUM(CASE WHEN OrderStatus = 'Cancelled' THEN 1 ELSE 0 END) AS cancelled,
  ROUND(
    SUM(CASE WHEN OrderStatus = 'Cancelled' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 1
  ) AS cancel_rate_pct
FROM orders
GROUP BY Product
ORDER BY cancel_rate_pct DESC;
```

---

> 💡 Queries written in **MySQL** syntax.  
> For PostgreSQL: replace `DATE_FORMAT(date, '%Y-%m')` with `TO_CHAR(date, 'YYYY-MM')`
