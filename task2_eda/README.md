# Task 2 — Exploratory Data Analysis (EDA)

## Key Metrics

| Metric | Value |
|--------|-------|
| Total Revenue | $1,264,762 |
| Total Orders | 1,200 |
| Average Order Value | $1,054 |
| Unique Customers | 1,200 |
| Cancellation Rate | 20.8% |

## Python Code

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_excel("Dataset_Cleaned.xlsx")

# ── 1. Revenue by Product ──────────────────────────────────────
product_revenue = df.groupby("Product")["TotalPrice"].agg(
    Total_Revenue="sum", Avg_Order="mean", Orders="count"
).sort_values("Total_Revenue", ascending=False)
print(product_revenue)

# ── 2. Order Status Distribution ──────────────────────────────
status_counts = df["OrderStatus"].value_counts()
print(status_counts)

# ── 3. Monthly Revenue Trend ──────────────────────────────────
df["YearMonth"] = df["Date"].dt.to_period("M")
monthly = df.groupby("YearMonth")["TotalPrice"].sum()
print(monthly)

# ── 4. Referral Source Analysis ───────────────────────────────
referral = df.groupby("ReferralSource").agg(
    Revenue=("TotalPrice", "sum"),
    Orders=("OrderID", "count")
).sort_values("Revenue", ascending=False)
print(referral)

# ── 5. Coupon Code Usage ──────────────────────────────────────
coupon = df["CouponCode"].value_counts()
print(coupon)

# ── 6. Payment Methods ────────────────────────────────────────
payment = df["PaymentMethod"].value_counts()
print(payment)

# ── 7. Correlation ────────────────────────────────────────────
print(df[["Quantity", "UnitPrice", "TotalPrice", "ItemsInCart"]].corr())
```

## Key Findings

### 📦 Revenue by Product
| Product | Orders | Total Revenue |
|---------|--------|---------------|
| Chair   | 178 | $195,620 |
| Printer | 181 | $195,613 |
| Laptop  | 173 | $192,127 |
| Tablet  | 179 | $186,569 |
| Monitor | 163 | $175,651 |
| Desk    | 170 | $167,460 |
| Phone   | 156 | $151,722 |

### 📊 Order Status
| Status | Count | % |
|--------|-------|---|
| Cancelled | 250 | 20.8% |
| Returned  | 247 | 20.6% |
| Pending   | 237 | 19.8% |
| Shipped   | 235 | 19.6% |
| Delivered | 231 | 19.3% |

### 📣 Referral Sources
| Source | Revenue |
|--------|---------|
| Instagram | $275,285 |
| Email     | $261,809 |
| Google    | $250,441 |
| Facebook  | $250,411 |
| Referral  | $226,816 |

### 🎟️ Coupon Usage
| Coupon | Usage |
|--------|-------|
| FREESHIP | 313 (26.1%) |
| WINTER15 | 292 (24.3%) |
| SAVE10   | 286 (23.8%) |
| No Coupon | 309 (25.8%) |

## Insights
> ⚠️ **41.4%** of orders are Cancelled or Returned — needs investigation  
> 📱 Instagram is the top revenue-driving channel  
> 💳 Payment methods are evenly distributed (no dominant method)  
> 🪑 Chair and Printer are virtually tied as top revenue products  
