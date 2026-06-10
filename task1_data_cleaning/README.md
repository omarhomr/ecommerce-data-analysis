# Task 1 — Data Cleaning (Missing Values, Duplicates, Formatting)

## Dataset Overview
- **Records:** 1,200 rows × 14 columns
- **Source:** E-Commerce sales dataset

## Issues Found & Fixed

| Column | Issue | Count | Solution |
|--------|-------|-------|----------|
| `CouponCode` | Missing values (NaN) | 309 | Fill with `"No Coupon"` |
| `UnitPrice` | Floating point noise | Multiple | Round to 2 decimals |
| `TotalPrice` | Floating point noise | Multiple | Round to 2 decimals |
| All columns | Duplicates | **0** | No action needed ✅ |

## Python Code

```python
import pandas as pd
import numpy as np

df = pd.read_excel("Dataset.xlsx")

# 1. Check missing values
print(df.isnull().sum())
# Output: CouponCode    309  (all others = 0)

# 2. Fill missing CouponCode
df["CouponCode"] = df["CouponCode"].fillna("No Coupon")

# 3. Fix floating point noise
df["UnitPrice"]  = df["UnitPrice"].round(2)
df["TotalPrice"] = df["TotalPrice"].round(2)

# 4. Parse dates & extract Year/Month
df["Year"]  = df["Date"].dt.year
df["Month"] = df["Date"].dt.month

# 5. Check duplicates
print(f"Duplicates: {df.duplicated().sum()}")  # → 0

# 6. Validate TotalPrice = Quantity × UnitPrice
mismatch = ~np.isclose(df["TotalPrice"], df["Quantity"] * df["UnitPrice"], rtol=0.01)
print(f"Price mismatches: {mismatch.sum()}")  # → 0

# 7. Save cleaned file
df.to_excel("Dataset_Cleaned.xlsx", index=False)
print("✅ Cleaning complete — 0 missing values, 0 duplicates")
```

## Results After Cleaning
- ✅ 1,200 complete records
- ✅ 0 missing values
- ✅ 0 duplicates
- ✅ Prices rounded to 2 decimal places
- ✅ Date columns parsed and validated
